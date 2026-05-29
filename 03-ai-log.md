Dưới đây là một bản thảo tự luận phản ánh thực tế quá trình làm việc với AI, được thiết kế sát với các tác vụ nghiên cứu học sâu và xử lý dữ liệu phức tạp. Bạn có thể sử dụng hoặc tinh chỉnh thêm cho phù hợp với phong cách cá nhân của mình.

### Nhật ký chiêm nghiệm: AI trong vai trò "Thought-Partner" và những giới hạn thực tế

**1. AI đã giúp tôi những gì?**
Trong suốt buổi nghiên cứu tinh chỉnh kiến trúc cho mô hình tương tác thuốc - đích (Drug-Target Interaction), AI thực sự là một người đồng hành (thought-partner) đắc lực. Thay vì chỉ dùng để viết code rập khuôn, tôi đã sử dụng AI để brainstorm các ý tưởng phức tạp, điển hình như việc tích hợp đặc trưng không gian (spatial features) và cấu trúc lại các kết nối thặng dư (residual connections) sao cho hiệu quả. AI hỗ trợ tôi phân tích ưu nhược điểm của việc trích xuất đặc trưng trực tiếp từ các mô hình ngôn ngữ lớn (LLMs) so với các phương pháp truyền thống. Bên cạnh đó, để tăng tốc quy trình, tôi thường xuyên nhờ AI viết các script Pandas phức tạp, đặc biệt là các thao tác liên quan đến window functions và gộp nhóm dữ liệu để chuẩn bị đầu vào cho mô hình.

**2. AI đã sai lệch ở đâu?**
Dù có khả năng tổng hợp ý tưởng tốt, AI bắt đầu bộc lộ rõ điểm yếu (hallucination) khi đi sâu vào logic toán học và cấu trúc không gian của các ma trận đa chiều.

Cụ thể, trong lúc gỡ lỗi cho một pipeline xử lý tensor, AI đã đề xuất sử dụng hàm `.view()` trong PyTorch để sắp xếp lại kích thước ma trận. Đề xuất này là một cái bẫy nguy hiểm: code vẫn chạy trơn tru, hoàn toàn không báo lỗi, nhưng thực chất hàm `.view()` đã làm xáo trộn và phá hỏng hoàn toàn các đặc trưng không gian (spatial semantics) của dữ liệu. Ngoài ra, khi được yêu cầu đề xuất chiến lược theo dõi chỉ số AUROC và AUPRC cho tập dữ liệu mất cân bằng, AI lại đưa ra một hệ thống rule-based sử dụng các vòng lặp `for` lồng nhau vô cùng phức tạp và phi thực tế, thay vì tận dụng khả năng tính toán vector hóa (vectorized operations), dẫn đến việc làm chậm toàn bộ quá trình huấn luyện đi hàng chục lần.

**3. Quá trình điều chỉnh và thiết lập ranh giới**
Nhận ra xu hướng "tự tin đưa ra giải pháp sai" của AI, tôi buộc phải thay đổi chiến thuật viết prompt. Thay vì sử dụng những câu lệnh mở kiểu như *"Làm sao để sửa lỗi này?"*, tôi chuyển sang cấu trúc prompt có ranh giới kỹ thuật chặt chẽ (rule-based constraints):

* **Ép buộc logic toán học:** Tôi bổ sung yêu cầu *"Tuyệt đối không dùng `.view()` để hoán đổi các chiều không gian. Bắt buộc sử dụng `.permute()` và phải chú thích rõ ràng kích thước (shape) của tensor trước và sau khi biến đổi ở mỗi dòng code."*
* **Giới hạn không gian giải pháp:** Để tránh các đề xuất vòng lặp thừa thãi khi tính toán AUPRC, tôi đặt ranh giới: *"Chỉ sử dụng các phép toán vector hóa của thư viện chuẩn hoặc Scikit-learn. Không sử dụng bất kỳ vòng lặp thuần Python nào trong hàm đánh giá."*
* **Kích hoạt cơ chế tự kiểm chứng:** Cuối mỗi prompt phức tạp, tôi thêm câu lệnh *"Hãy tự đóng vai một kỹ sư phản biện. Tìm ra ít nhất một rủi ro tiềm ẩn (ví dụ: memory leak, sai lệch chiều dữ liệu) trong đoạn code bạn vừa đề xuất và tự sửa nó trước khi trả kết quả cuối cùng."*

Nhờ việc siết chặt các ranh giới này, AI không còn sinh ra các đoạn code "ảo giác" và cung cấp các giải pháp an toàn, tối ưu hơn hẳn. Trải nghiệm này là một lời nhắc nhở thực tế: AI là một công cụ tuyệt vời để mở rộng giới hạn tư duy, nhưng người nắm giữ logic cốt lõi và chịu trách nhiệm kiểm chứng cuối cùng vẫn bắt buộc phải là chính mình.