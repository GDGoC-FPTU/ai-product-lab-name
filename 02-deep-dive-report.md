# THÔNG TIN THÀNH VIÊN NHÓM 
**Tên nhóm:** name 
**Thành viên:**
1. Trần Minh Anh - 2A202600706
2. 

****
# 🏗️ Phase 3 — DEEP-DIVE (Nhóm)

## Bài toán:

### Vinmec — AI Tự động hóa tóm tắt hồ sơ xuất viện (Discharge Summary)

---

# 3.1. Current-State Workflow Mapping

## Quy trình vận hành hiện tại

```text id="workflow-current"
                         QUY TRÌNH THỦ CÔNG HIỆN TẠI

┌──────────────────────────────────────────────────────────────────────────┐
│ 1. Bác sĩ đọc toàn bộ hồ sơ bệnh án trên HIS/EHR                       │
│    - Đọc diễn tiến điều trị                                             │
│    - Xem kết quả Lab/PACS                                               │
│    - Kiểm tra toa thuốc                                                 │
│                                                                          │
│    ⏱ ~8–12 phút                                                         │
└──────────────────────────────────────────────────────────────────────────┘
                               │
                               │ 🔄 HANDOFF
                               ▼
┌──────────────────────────────────────────────────────────────────────────┐
│ 2. Sàng lọc thủ công dữ liệu lâm sàng                                  │
│    - Chọn xét nghiệm quan trọng                                         │
│    - Xác định chẩn đoán chính/phụ                                       │
│    - Tổng hợp timeline điều trị                                         │
│                                                                          │
│    🔴 BOTTLENECK                                                         │
│    Dữ liệu phân mảnh, dễ bỏ sót thông tin                               │
│                                                                          │
│    ⏱ ~12–18 phút                                                        │
└──────────────────────────────────────────────────────────────────────────┘
                               │
                               │ 🔄 HANDOFF
                               ▼
┌──────────────────────────────────────────────────────────────────────────┐
│ 3. Gõ tay discharge summary                                             │
│    - Viết narrative lâm sàng                                            │
│    - Mô tả phương pháp điều trị                                         │
│    - Tổng hợp kết quả điều trị                                          │
│                                                                          │
│    🔴 BOTTLENECK                                                         │
│    Tốn thời gian nhập liệu và chuẩn hóa ngôn ngữ                        │
│                                                                          │
│    ⏱ ~13–17 phút                                                        │
└──────────────────────────────────────────────────────────────────────────┘
                               │
                               │ 🔄 HANDOFF
                               ▼
┌──────────────────────────────────────────────────────────────────────────┐
│ 4. Soạn hướng dẫn dùng thuốc & tái khám                                │
│                                                                          │
│    ⏱ ~2–5 phút                                                          │
└──────────────────────────────────────────────────────────────────────────┘

TOTAL TIME = ~35–52 phút / bệnh nhân
```

---

# 3.2. Problem Statement (6-field) & Metrics

| Field                       | Nội dung chi tiết                                                                                                                                                                        |
| --------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **1. Actor / Operator**     | Bác sĩ điều trị, bác sĩ nội trú và điều dưỡng hỗ trợ thủ tục xuất viện                                                                                                                   |
| **2. Current Workflow**     | Bác sĩ đọc dữ liệu từ HIS/EHR, PACS, Lab rồi tự tổng hợp chẩn đoán, xét nghiệm và viết tóm tắt hồ sơ bệnh án thủ công trên hệ thống HIS                                                      |
| **3. Bottleneck**           | Bước sàng lọc dữ liệu y khoa và viết diễn biến lâm sàng là bước chậm và dễ sai sót nhất do dữ liệu dài, phân mảnh và không đồng nhất                                                     |
| **4. Business Impact**      | Mỗi ca nội trú mất ~35–52 phút xử lý; mỗi khoa có thể mất khoảng 20 giờ công việc hành chính/ngày; làm chậm quá trình xuất viện và tăng workload cho bác sĩ                      |
| **5. Success Metric**       | - Giảm thời gian tóm tắt xuống < 4 phút/ca <br> - ≥ 85% draft được bác sĩ chấp nhận sau chỉnh sửa nhẹ <br> - AI phản hồi trong < 30 giây                                   |
| **6. Operational Boundary** | AI chỉ được phép tạo bản nháp, tóm tắt và hỗ trợ trích xuất dữ liệu. AI không được tự chẩn đoán, tự kê đơn hoặc tự ký discharge summary. Bác sĩ bắt buộc review trước khi lưu chính thức |

---

# 3.3. Future-State Flow & AI Fit

## AI-Fit Matrix

* [ ] Rule / State-Machine
* [x] LLM Feature
* [ ] Agentic Loop

## Giải thích AI Fit

Bài toán phù hợp với mô hình LLM Feature vì:

* dữ liệu chủ yếu là văn bản y khoa phi cấu trúc,
* cần khả năng đọc hiểu và tóm tắt dài,
* cần sinh narrative lâm sàng theo format chuẩn,
* workflow vẫn cần bác sĩ kiểm duyệt nên chưa cần Agentic AI.

---

## Future-State Flow

```text id="workflow-future"
┌──────────────────────────────────────────────────────────────┐
│ 1. Bác sĩ chọn "Generate Discharge Summary"                 │
│                                                              │
│ 🟢 HUMAN STEP                                                │
└──────────────────────────────────────────────────────────────┘
                            │
                            ▼
┌──────────────────────────────────────────────────────────────┐
│ 2. AI truy xuất dữ liệu từ HIS/EHR, PACS và Lab             │
│                                                              │
│ 🔵 AI STEP                                                   │
└──────────────────────────────────────────────────────────────┘
                            │
                            ▼
┌──────────────────────────────────────────────────────────────┐
│ 3. AI trích xuất dữ liệu y khoa                             │
│    - Chẩn đoán                                               │
│    - Xét nghiệm bất thường                                   │
│    - Thuốc và timeline điều trị                             │
│                                                              │
│ 🔵 AI STEP                                                   │
└──────────────────────────────────────────────────────────────┘
                            │
                            ▼
┌──────────────────────────────────────────────────────────────┐
│ 4. AI sinh bản nháp discharge summary                       │
│    - Narrative lâm sàng                                     │
│    - Hướng dẫn dùng thuốc                                   │
│    - Dặn dò tái khám                                        │
│                                                              │
│ 🔵 AI STEP                                                   │
└──────────────────────────────────────────────────────────────┘
                            │
                            ▼
┌──────────────────────────────────────────────────────────────┐
│ 5. Bác sĩ review, chỉnh sửa và approve                      │
│                                                              │
│ 🟢 HUMAN STEP (HITL)                                         │
└──────────────────────────────────────────────────────────────┘
                            │
             ┌──────────────┴──────────────┐
             │                             │
             ▼                             ▼
      [Approved]                    [Low Confidence]
             │                             │
             ▼                             ▼
┌───────────────────────┐     ┌──────────────────────────────┐
│ 6. Lưu HIS & xuất viện│     │ ↩️ FALLBACK                   │
│                       │     │ - Chuyển sang nhập tay        │
│ 🟢 HUMAN + SYSTEM     │     │ - Highlight phần AI unsure    │
└───────────────────────┘     │ - Yêu cầu bác sĩ bổ sung      │
                               └──────────────────────────────┘
```

---

# 🏁 Phase 5 — EVALUATE

## AI Readiness Checklist

1. [x] Chúng tôi có sẵn dữ liệu mẫu/logs sạch để test?
   → Vinmec đã có dữ liệu từ HIS/EHR, PACS và Lab để pilot.

2. [x] Rủi ro khi AI sai có nằm trong tầm kiểm soát?
   → Có Human-in-the-loop và fallback manual workflow.

3. [ ] Stakeholders sẵn sàng thay đổi quy trình làm việc cũ?
   → Cần training và pilot trước khi rollout toàn hệ thống.

---

# Quyết định cuối cùng của Ban Giám Đốc Vin Smart Future

* [x] GO (Bắt đầu xây dựng Prototype)
* [ ] NOT YET
* [ ] NO-GO

---

# Justification (Lý giải quyết định)

Dự án có AI-fit cao vì:

* dữ liệu đầu vào chủ yếu là văn bản y khoa phi cấu trúc,
* workflow hiện tại phụ thuộc nhiều vào thao tác đọc hiểu và viết narrative thủ công,
* LLM hiện đại đã có khả năng mạnh trong medical summarization và entity extraction.

Ngoài ra:

* output discharge summary có format tương đối chuẩn hóa,
* bệnh viện đã có historical records để làm evaluation,
* quy trình có HITL nên rủi ro hallucination nằm trong vùng kiểm soát được.

Về business impact:

* giảm thời gian xử lý từ ~35–52 phút xuống dưới 4 phút/ca,
* giảm overtime bác sĩ,
* tăng tốc độ discharge,
* tăng trải nghiệm bệnh nhân,
* giúp bác sĩ dành nhiều thời gian hơn cho clinical care thay vì paperwork.

Do đó, nhóm đề xuất:

* triển khai prototype với phạm vi hẹp,
* pilot trước tại một khoa nội trú,
* đo KPI thực tế trước khi mở rộng toàn hệ thống.
