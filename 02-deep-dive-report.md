# 02 — Deep-Dive Report

**Nhóm:** The Hive
**Bài toán chọn:** Xanh SM (GSM) — Xử lý sự cố pin thực địa (Card #1, xem `01-problem-scan.md`)

**Thành viên:**
- Đoàn Xuân Duy Bảo — doanxuanduybao96@gmail.com
- Triệu Quốc Khánh — trieukhanh25032005@gmail.com
- Vũ Quốc Đạt — dvuquoc60@gmail.com
- Vũ Tiến Hùng — hungvu.education@gmail.com
- Hoàng Đức An — anduchoang2701@gmail.com

---

## 3.1. Current-State Workflow Mapping

Quy trình thủ công hiện tại khi tài xế Xanh SM báo sự cố hết/cạn pin giữa đường:

```text
┌──────────────┐     ┌──────────────┐     ┌──────────────┐     ┌──────────────┐
│ Bước 1       │     │ Bước 2       │     │ Bước 3       │     │ Bước 4       │
│ Nhận cuộc    │     │ Tra cứu định │     │ Tra cứu trạm │     │ Soạn văn bản │
│ gọi sự cố    │ ──→ │ vị GPS xe   │ ──→ │ sạc VinFast  │ ──→ │ hướng dẫn    │
│              │     │              │     │ còn trụ trống│     │ gửi tài xế   │
│ Ai: Dispatch │     │ Ai: Dispatch │     │ Ai: Dispatch │     │ Ai: Dispatch │
│ ⏱ ~2 phút    │     │ ⏱ ~2 phút    │     │ ⏱ ~5 phút 🔴 │     │ ⏱ ~5 phút 🔴 │
│ In: Điện thoại│     │ In: Biển số  │     │ In: Vị trí GPS│     │ In: Raw data │
│ Out: Log sự cố│     │ Out: Toạ độ  │     │ Out: Địa chỉ │     │ Out: SMS     │
└──────────────┘     └──────────────┘     └──────────────┘     └──────────────┘
                                                                      │
                                                                      ▼
                                                               ┌──────────────┐
                                                               │ Bước 5       │
                                                               │ 🔄 Handoff:  │
                                                               │ Gọi xe cứu   │
                                                               │ hộ (nếu cần) │
                                                               │ Ai: Dispatch │
                                                               │ ⏱ ~1 phút    │
                                                               └──────────────┘

🔴 Bottleneck: Bước 3-4 (tra trụ sạc phù hợp dòng xe + soạn chỉ dẫn chi tiết, tiếng Việt thân thiện)
🔄 Handoff: Bước 1→2 (điện thoại → hệ thống định vị), Bước 4→5 (tin nhắn → đội cứu hộ nếu AI/điều phối phát hiện pin quá thấp)

⏱ TỔNG THỜI GIAN XỬ LÝ THỦ CÔNG: ~15 phút/lượt
⚠️ Lưu ý: số thời gian trên là ƯỚC LƯỢNG CHƯA KIỂM CHỨNG (mô phỏng theo quy trình hợp lý, chưa
phỏng vấn điều phối viên thật hoặc trích log hệ thống Xanh SM). Nhóm cần xác thực số liệu thật ở
Phase 4 (Validation + Research) trước khi chốt vào Problem Statement v1.
```

---

## 3.2. Problem Statement (6 trường)

| Field | Nội dung |
|---|---|
| **1. Actor / Operator** | Điều phối viên (Dispatcher) thuộc Trung tâm Điều vận Xanh SM; đối tượng chịu ảnh hưởng gián tiếp là tài xế đang chờ xử lý sự cố. |
| **2. Current Workflow** | Khi tài xế báo hết pin, điều phối viên: (1) nhận cuộc gọi, (2) tra vị trí GPS xe trên bản đồ nội bộ, (3) mở dashboard trạm sạc VinFast để tìm trụ trống gần nhất phù hợp dòng xe, (4) soạn tin nhắn chỉ dẫn gửi qua App tài xế, (5) gọi xe cứu hộ nếu pin dưới ngưỡng an toàn. 5 bước, hoàn toàn thủ công. |
| **3. Bottleneck** | Bước 3 & 4: tra cứu thủ công trụ sạc trống đúng loại cổng sạc (CCS2/GB-T) theo dòng xe (VF5/VFe34/VF8/VF9), và soạn thảo tin nhắn hướng dẫn đường đi bằng tiếng Việt rõ ràng, dễ hiểu cho tài xế đang gấp. |
| **4. Business Impact** | Mỗi sự cố tốn ~10 phút thao tác thủ công của điều phối viên ở bước nghẽn; nhân lực điều vận bị chiếm dụng trong giờ cao điểm, kéo dài thời gian tài xế chờ → tăng rủi ro hủy chuyến/khách chờ lâu. Xem dự báo tác động định lượng ở mục **3.4** bên dưới. *(Số liệu quy mô thật — bao nhiêu sự cố/ngày tại Hà Nội — cần nhóm xác thực ở Phase 4, hiện mới là ước tính từ benchmark công khai.)* |
| **5. Success Metric** | (a) Giảm thời gian xử lý sự cố từ ~15 phút xuống dưới 5 phút/lượt (Efficiency). (b) Tỉ lệ hướng dẫn đúng địa điểm + đúng loại trụ sạc đạt ≥95% (Quality). *(Baseline "15 phút" là ước lượng — cần số thật trước khi tính vào bản v1.)* |
| **6. Operational Boundary** | AI được phép: truy xuất API định vị xe, API trạm sạc VinFast còn trống, tự động soạn **draft** tin nhắn hướng dẫn. **CẤM:** AI không được tự động gửi tin đi mà không có điều phối viên phê duyệt (bắt buộc HITL); không được đề xuất trạm sạc cách xe >5km khi pin báo <5% (rủi ro xe cạn pin giữa đường) — trường hợp này phải tự động đề xuất điều xe cứu hộ pin di động thay vì chỉ đường. |

---

## 3.3. Future-State Flow & AI Fit

**AI-Fit:** chọn **LLM Feature** (không chọn Agent tự trị — quy trình có cấu trúc cố định, rủi ro nếu AI tự quyết định sai trạm sạc có thể khiến xe cạn pin giữa đường, gây nguy hiểm/tắc nghẽn giao thông; không chọn Rule-based vì cần xử lý ngôn ngữ tự nhiên để soạn chỉ dẫn linh hoạt theo từng tình huống).

```text
┌──────────────┐     ┌──────────────┐     ┌──────────────┐     ┌──────────────┐
│ Bước 1       │     │ Bước 2       │     │ Bước 3       │     │ Bước 4       │
│ Nhận cuộc    │     │ 🔵 Auto-pull │     │ 🔵 AI draft  │     │ 🟢 Dispatch  │
│ gọi sự cố    │ ──→ │ vị trí &     │ ──→ │ SMS chỉ dẫn  │ ──→ │ review & gửi │
│              │     │ trạm sạc trống│    │ & chỉ đường  │     │ cho tài xế   │
└──────────────┘     └──────────────┘     └──────────────┘     └──────────────┘
                                                                      │
                                                                      ▼
                                                               ↩️ Fallback:
                                                               Nếu AI draft lỗi/
                                                               không chắc chắn,
                                                               điều phối viên tự
                                                               viết tay như cũ.

🔵 AI Step: Bước 2 (tự động tra GPS + trạm sạc qua API), Bước 3 (AI soạn draft tin nhắn)
🟢 Human/HITL: Bước 4 (điều phối viên review nội dung draft trước khi gửi — không bỏ qua bước này
   với mọi mức pin, đặc biệt khi pin <5% theo Operational Boundary)
↩️ Fallback: nếu AI không tìm được trạm sạc phù hợp, hoặc draft chứa cảnh báo bất thường → chuyển
   thẳng về quy trình thủ công cũ (điều phối viên tự tra + tự viết)
```

**Kỳ vọng cải thiện:** rút thời gian xử lý bước 3-4 từ ~10 phút xuống còn vài giây (AI tự động tra + draft), điều phối viên chỉ cần đọc lướt + duyệt gửi (~1 phút) — tổng quy trình có thể về dưới 5 phút/lượt.

---

## 3.4. Dự báo tác động kinh doanh (Business Impact Forecast)

Theo yêu cầu của giảng viên: mọi ý tưởng phải được quy về số liệu. Nhóm không có dữ liệu nội bộ Xanh SM, nên dùng **benchmark công khai** làm cơ sở ước tính — số liệu dưới đây là **ước tính bậc độ lớn, có nguồn trích dẫn, không phải cam kết hiệu quả**.

**Input benchmark (có nguồn):**
- Quy mô đội xe: GSM/Xanh SM có **>30.000 xe taxi điện toàn quốc**, chiếm **54,5% thị phần taxi công nghệ** tại Việt Nam Q1/2026 (nguồn: Mordor Intelligence, dẫn lại qua [greensm.com](https://www.greensm.com/vn-vi/news/chiem-gan-45-thi-phan-xanh-sm-ngay-cang-vuot-xa-cac-doi-thu-tren-thi-truong)).
- Lương điều phối viên/tổng đài viên tại Việt Nam: phổ biến **7-15 triệu VNĐ/tháng** (nguồn: [JobsGO](https://jobsgo.vn/viec-lam-dieu-phoi-vien-tai-ho-chi-minh.html)), lấy trung vị **10 triệu VNĐ/tháng ≈ 45.000đ/giờ công** (giả định ~220 giờ công/tháng).

**Giả định tính toán (cần Xanh SM xác nhận ở Phase 4):**
- Tỷ lệ ca vận hành/ngày phát sinh sự cố pin cần điều phối hỗ trợ: **0,5-1%**.
- Thời gian xử lý thủ công hiện tại ở bước nghẽn (3-4): **~10 phút/sự cố**.
- AI giúp giảm **~80%** thời gian ở bước nghẽn (điều phối viên chỉ cần đọc + duyệt).

**Kết quả ước tính:**

| Chỉ số | Ước tính |
|---|---|
| Số sự cố/ngày toàn quốc | 150–300 sự cố (0,5-1% × 30.000 xe) |
| Thời gian công điều phối tiêu tốn/ngày | 25–50 giờ công (150-300 sự cố × 10 phút) |
| **Tiết kiệm thời gian** nếu áp dụng AI | ~20–40 giờ công/ngày (giảm 80%) |
| **Tiết kiệm chi phí** (giờ công × 45.000đ/giờ) | ~**35–70 triệu VNĐ/tháng** toàn quốc |
| **Giảm rủi ro** | Giảm nguy cơ xe cạn pin giữa đường (tắc nghẽn giao thông, mất an toàn) + giảm tỷ lệ hủy chuyến do tài xế chờ lâu → **bảo toàn doanh thu cuốc xe** (chưa định lượng được vì thiếu số liệu tỷ lệ hủy chuyến thật) |
| **Tăng doanh thu gián tiếp** | Tài xế được giải cứu nhanh hơn → sớm quay lại đón khách → tăng số cuốc/ngày/xe (chưa định lượng, cần dữ liệu vận hành thật) |

**Diễn giải theo 3 nhóm lợi ích doanh nghiệp (theo đúng khung thầy yêu cầu):**
1. **Tiết kiệm thời gian:** ~20-40 giờ công điều phối/ngày toàn quốc.
2. **Tiết kiệm/tăng doanh thu:** ~35-70 triệu VNĐ/tháng tiền công tiết kiệm trực tiếp + doanh thu cuốc xe được bảo toàn gián tiếp (chưa định lượng được phần này do thiếu baseline tỷ lệ hủy chuyến thật).
3. **Giảm rủi ro:** giảm rủi ro an toàn giao thông (xe cạn pin giữa đường) và rủi ro vận hành (điều phối viên quá tải giờ cao điểm dẫn đến xử lý sai/chậm các ca khác).

> ⚠️ Toàn bộ số liệu trên là **ước tính minh họa dựa trên benchmark công khai**, phục vụ mục đích so sánh/ra quyết định trong bài lab — **không phải số liệu tài chính chính thức của Xanh SM**. Trước khi đưa vào Problem Statement v1 (Phase 6), nhóm cần Xanh SM xác nhận: (a) tỷ lệ sự cố pin thật/ngày, (b) thời gian xử lý thật qua log hệ thống, (c) cơ cấu lương thật của điều phối viên.

---

## Phase 5 — Evaluate

### AI Readiness Checklist
- [ ] Có sẵn dữ liệu mẫu/log sự cố thật để test? *(chưa — cần Xanh SM cung cấp hoặc nhóm giả lập theo kịch bản thực tế)*
- [x] Rủi ro khi AI sai nằm trong tầm kiểm soát (qua HITL + Fallback)? *(có — bước 4 bắt buộc người duyệt, có fallback về thủ công)*
- [ ] Stakeholders (điều phối viên) sẵn sàng thay đổi quy trình làm việc cũ? *(chưa khảo sát thật, giả định có vì giảm tải công việc cho họ)*

### Quyết định của nhóm
**[ ] GO** &nbsp;&nbsp; **[x] NOT YET** &nbsp;&nbsp; **[ ] NO-GO**

**Lý giải:** Bài toán rõ ràng, Operational Boundary kiểm soát được rủi ro, AI-Fit (LLM Feature) phù hợp. Tuy nhiên **chưa đủ căn cứ để chọn GO ngay** vì: (1) toàn bộ số liệu baseline (10 phút, 15 phút, số sự cố/ngày) đang là ước lượng, chưa có dữ liệu thật từ Xanh SM; (2) chưa có bộ dữ liệu mẫu/log sự cố thật để thiết kế test case cho Phase 4 (Stress-test Prompt). Khuyến nghị: **Not Yet** — tạm hoãn để (a) thu thập baseline thật (phỏng vấn điều phối viên hoặc log mẫu), (b) sau đó quay lại chốt Go ở Problem Statement v1. Quyết định này không phải thất bại mà là đúng nguyên tắc "Pain point phải được lượng hóa" trong bài giảng Day 02.

> *Nhóm có thể đổi thành GO nếu trong buổi họp xác nhận được số liệu baseline gần đúng từ kinh nghiệm thực tế của một thành viên, hoặc nếu giảng viên/mentor xác nhận ước lượng hiện tại đủ dùng cho mục đích bài lab.*
