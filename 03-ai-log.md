# 📝 Phase 6 — REFLECTION (Cá nhân)
**Họ và tên:** Trần Minh Anh   
**Mã số sinh viên (MSSV):** 2A202600706 

# Nhật ký chiêm nghiệm về việc tương tác với AI trong buổi học

Trong suốt buổi học, em sử dụng AI như một “thought-partner” thay vì chỉ xem AI là công cụ trả lời câu hỏi đơn thuần. Em chủ yếu sử dụng ChatGPT để brainstorm ý tưởng, phân tích workflow nghiệp vụ và hỗ trợ cấu trúc bài làm theo format yêu cầu của môn học.

Ở giai đoạn đầu, AI giúp em rất nhiều trong việc mở rộng góc nhìn về các pain point vận hành tại các công ty thuộc Vingroup như Vinmec, VinFast và Xanh SM. Ban đầu em chỉ nghĩ đến những bài toán khá chung chung như chatbot chăm sóc khách hàng hoặc hệ thống hỏi đáp. Tuy nhiên, khi mô tả chi tiết hơn về workflow thực tế, AI đã gợi ý các vấn đề có giá trị vận hành cao hơn như:

* tự động hóa discharge summary tại Vinmec,
* AI routing triệu chứng bệnh nhân,
* phân tích call logs của Xanh SM,
* tự động phân loại ticket bảo hành tại VinFast.

Điều em thấy hữu ích nhất là AI có thể nhanh chóng chuyển một ý tưởng mơ hồ thành cấu trúc business problem khá hoàn chỉnh gồm:

* actor,
* workflow,
* bottleneck,
* business impact,
* metric,
* AI-fit.

Ngoài brainstorming, em còn dùng AI để:

* chỉnh sửa wording cho chuyên nghiệp hơn,
* format markdown,
* vẽ workflow dạng text diagram,
* đề xuất KPI định lượng,
* phân loại use-case theo Rule-based / LLM Feature / Agentic Loop.

Tuy nhiên, trong quá trình làm việc, AI cũng đưa ra một số câu trả lời chưa chính xác hoặc hơi “ảo tưởng năng lực AI” (hallucination).

Ví dụ, ở một lần prompt ban đầu, AI đề xuất dùng “Agentic AI” cho bài toán discharge summary của Vinmec. Nhưng sau khi phân tích kỹ hơn, em nhận ra workflow này thực chất chỉ cần:

* đọc hiểu hồ sơ,
* trích xuất thông tin,
* sinh bản nháp văn bản.

Nó chưa cần khả năng autonomous planning hay multi-step tool execution của Agentic AI. Nếu áp dụng agent quá sớm sẽ làm hệ thống phức tạp không cần thiết, khó kiểm soát và tăng rủi ro hallucination trong môi trường y tế. Sau đó em sửa prompt bằng cách bổ sung ranh giới:

* “workflow vẫn cần bác sĩ approve”,
* “AI chỉ tạo draft thay vì tự ra quyết định”,
* “ưu tiên giải pháp đơn giản nhất có thể”.

Sau khi thêm các boundary này, AI bắt đầu trả lời hợp lý hơn và chuyển hướng sang kiến trúc “LLM Feature + Human-in-the-loop”.

Một trường hợp khác là AI đôi lúc đưa ra metric hơi phi thực tế như:

* “100% routing accuracy”,
* “0 lỗi hallucination”.

Em nhận ra nếu prompt không có ràng buộc business reality thì AI thường có xu hướng tối ưu hóa quá mức hoặc đưa ra KPI quá đẹp. Vì vậy em đã sửa prompt theo hướng:

* yêu cầu metric thực tế,
* bắt buộc có fallback,
* phải mô tả risk khi AI sai,
* thêm HITL (Human-in-the-loop).

Điều này giúp output cân bằng hơn giữa góc nhìn kỹ thuật và vận hành thực tế.

Qua buổi học, em nhận ra AI hoạt động hiệu quả nhất khi:

1. Con người mô tả rõ context nghiệp vụ.
2. Có boundary và constraint cụ thể.
3. Không “blind trust” vào câu trả lời đầu tiên.
4. Liên tục refine prompt giống quá trình review requirement trong software engineering.

Em cũng nhận ra vai trò của AI Engineer không chỉ là gọi API model, mà còn là:

* thiết kế workflow,
* kiểm soát rủi ro hallucination,
* xác định operational boundary,
* xây dựng HITL và fallback,
* đánh giá AI-fit thay vì cố “AI hóa” mọi thứ.

Sau buổi học, em thấy AI giống một cộng sự brainstorming tốc độ cao hơn là một hệ thống có thể tự thay thế hoàn toàn tư duy của kỹ sư.
