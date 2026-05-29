# Phase 1 & 2 — Problem Scan & Quick Cards (Cá nhân)

* **Họ và tên:** Trần Minh Anh   
* **Mã số sinh viên (MSSV):** 2A202600706 

---

# 🔍 Phase 1 — SCAN: Tìm kiếm cơ hội

Dưới đây là danh sách 5 bài toán/bottleneck thực tế được quét qua các công ty thành viên thuộc Tập đoàn Vingroup bằng cách áp dụng bộ **4 Lenses** công nghệ:

| # | Subsidiary | Lens | Mô tả ngắn bài toán & Tổn thất vận hành |
|---|------------|------|-----------------------------------------|
| 1 | **Vinmec** | Lặp lại, Tốn thời gian | Bác sĩ mất từ 25-40 phút cho mỗi bệnh nhân nội trú để đọc toàn bộ dữ liệu phân mảnh (EHR, PACS, Lab), sàng lọc chỉ số và gõ tay văn bản tóm tắt xuất viện bằng ngôn ngữ lâm sàng. Ước tính lãng phí 20 giờ làm việc/ngày tại mỗi khoa. |
| 2 | **VinFast** | Lặp lại, Tốn thời gian | Nhân viên tiếp nhận bảo hành đọc email/transcript rồi gõ tay ticket: tra mã lỗi, phân loại ưu tiên P1–P3 (~5 phút/ticket × 600 ticket/ngày). |
| 3 | **Xanh SM** | Pain từ người khác | Tài xế phàn nàn hết chuyến không biết di chuyển đâu chờ cuốc tiếp — không có gợi ý thông minh, idle time ước tính ~25% ca làm việc. |
| 4 | **VinFast** | AI có thể tốt hơn | Nhân viên CSKH soạn tay phản hồi từng đánh giá 1–2 sao của cư dân (~12 phút/phản hồi × 40 phản hồi/ngày toàn hệ thống, thường trễ 1–3 ngày). |
| 5 | **Xanh SM** | Tốn thời gian | Đội ngũ QA phải nghe lại thủ công hàng nghìn file ghi âm cuộc gọi hủy chuyến của khách hàng và ghi chú của tài xế để phân loại các pattern lỗi hệ thống gây rò rỉ doanh thu cuốc xe giờ cao điểm. |

---

# 🃏 Phase 2 — QUICK-ASSESS: 3 Quick Problem Cards

Dựa trên danh sách SCAN, dưới đây là 3 thẻ bài toán tiềm năng nhất được chọn lọc để cấu trúc hóa quy trình nghiệp vụ:

## QUICK PROBLEM CARD #1: Vinmec Tự động hóa tóm tắt hồ sơ xuất viện (Discharge Summary)

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1                                       │
│                                                             │
│ Bài toán: Bác sĩ nội trú mất quá nhiều thời gian viết tay   │
│ bản tóm tắt bệnh án lâm sàng khi làm thủ tục xuất viện.     │
│ Công ty thành viên: [x] Vinmec                              │
│                                                             │
│ Ai đang đau (Actor)? Bác sĩ điều trị và bệnh nhân ra viện.  │
│                                                             │
│ Workflow thủ công hiện tại (4 bước):                        │
│   1. Đọc toàn bộ lịch sử bệnh án trên hệ thống HIS nội bộ.  │
│   ──> 2. Sàng lọc thủ công các chỉ số xét nghiệm, chẩn đoán. │
│   ──> 3. Gõ tay diễn tiến bệnh lý và phương pháp điều trị.   │
│   ──> 4. Soạn thảo hướng dẫn dùng thuốc và dặn dò tái khám.  │
│                                                             │
│ Bước nào tốn nhất? Bước 2 & 3 (⏱ 25 - 35 phút/bệnh nhân)     │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2, 3 và 4         │
│ (Trích xuất thực thể y khoa ──> Tự động tạo bản nháp tóm tắt)│
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                        │
│ Giảm tổng thời gian biên soạn từ 35 phút ──> dưới 4 phút/ca. │
│                                                             │
│ Quick Architecture: [x] LLM Feature (Tạo bản nháp lâm sàng) │
└─────────────────────────────────────────────────────────────┘
```
