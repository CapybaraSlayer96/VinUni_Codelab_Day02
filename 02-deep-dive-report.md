# 02-deep-dive-report

**Tên nhóm:** The Hive

**Thành viên:**

1. Đoàn Xuân Duy Bảo - doanxuanduybao96@gmail.com
2. Triệu Quốc Khánh - trieukhanh25032005@gmail.com
3. Vũ Quốc Đạt - dvuquoc60@gmail.com
4. Vũ Tiến Hùng - hungvu.education@gmail.com
5. Hoàng Đức An - anduchoang2701@gmail.com

---

## Phase 2 – Quick Problem Cards (1 card/người)

### Quick Problem Card #1 – Bảo

```
QUICK PROBLEM CARD #1 (Bảo)

Bài toán (1 câu): Chuyển trung tâm vận hành đô thị từ giám sát theo ngưỡng sang dự báo phụ tải điện/nước, phát hiện bất thường và đề xuất lịch vận hành tối ưu.
Công ty thành viên: [ ] VinFast  [ ] Xanh SM  [x] Vinhomes  [ ] Vinmec  [ ] Khác

Ai đang đau (Actor)? Nhân viên trung tâm vận hành đô thị và kỹ thuật (Operations, Engineering) tại các khu đô thị.

Workflow thủ công hiện tại (3-5 bước):
  1. Thu thập dữ liệu meter/SCADA/BMS ──> 2. Theo dõi tiêu thụ theo tòa nhà, khu vực, thiết bị ──> 3. So với định mức/ngưỡng và lịch sử ──> 4. Tìm nguyên nhân (tải, thời tiết, lịch vận hành, rò rỉ) ──> 5. Điều chỉnh lịch vận hành

Bước nào tốn thời gian/lỗi nhất? Bước 3–4: phát hiện bất thường và tìm nguyên nhân chỉ dựa trên ngưỡng cảnh báo (⏱ chưa có số, cần time study).
AI có thể nhảy vào hỗ trợ ở bước nào? Bước 3–5: dự báo phụ tải, phát hiện bất thường, đề xuất lịch vận hành; người duyệt, không tự điều khiển thiết bị.

Đo thành công bằng gì (Metric có số)?
  Giảm 1–3% điện năng so với baseline chuẩn hóa theo thời tiết/occupancy
  ──> 8,1–24,2 tỷ VND/năm (322,3 triệu kWh × 2.500 VND/kWh, giá điện là giả định).

Quick Architecture: [ ] No AI  [x] Rule  [ ] LLM  [ ] Agent
```

### Quick Problem Card #2 – Khánh

```
QUICK PROBLEM CARD #2 (Khánh)

Bài toán (1 câu): Chatbot CSKH kịch bản cố định không trả lời được câu hỏi ngoài kịch bản, khách phải chờ chuyển nhân viên; dùng LLM trả lời theo chính sách/FAQ chính thức.
Công ty thành viên: [ ] VinFast  [x] Xanh SM  [ ] Vinhomes  [ ] Vinmec  [ ] Khác

Ai đang đau (Actor)? Khách hàng Xanh SM (chờ lâu) và nhân viên CSKH/tổng đài 1555 (nhận các câu hỏi chatbot không xử lý được).

Workflow thủ công hiện tại (3-5 bước):
  1. Khách hỏi qua chatbot/tổng đài ──> 2. Chatbot kịch bản không khớp câu hỏi ──> 3. Chuyển sang nhân viên ──> 4. Khách chờ ──> 5. Nhân viên tra chính sách, trả lời

Bước nào tốn thời gian/lỗi nhất? Bước 3–4: chuyển nhân viên và thời gian khách chờ (⏱ chưa có số, chưa có baseline).
AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2–5: LLM đọc câu hỏi tự do, trả lời theo FAQ/chính sách; case nhạy cảm (hoàn tiền, an toàn) hoặc không chắc thì chuyển người.

Đo thành công bằng gì (Metric có số)?
  Tỷ lệ tự giải quyết, thời gian phản hồi, CSAT. Mục tiêu số chốt sau khi có baseline.
  Quy mô tham chiếu: ~0,9 triệu chuyến/ngày (tự tính), giả định 1% chuyến phát sinh liên hệ CSKH ──> ~9.000 lượt/ngày.

Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM  [ ] Agent
```

### Quick Problem Card #3 – Đạt

```
QUICK PROBLEM CARD #3 (Đạt)

Bài toán (1 câu): Hỗ trợ điều phối viên xử lý sự cố hết/cạn pin của tài xế Xanh SM giữa đường: tìm trụ sạc phù hợp và soạn chỉ dẫn nhanh hơn.
Công ty thành viên: [ ] VinFast  [x] Xanh SM  [ ] Vinhomes  [ ] Vinmec  [ ] Khác

Ai đang đau (Actor)? Điều phối viên (dispatcher) Trung tâm Điều vận; tài xế phải chờ, mất cuốc.

Workflow thủ công hiện tại (3-5 bước):
  1. Nhận báo sự cố ──> 2. Tra GPS, biển số ──> 3. Tìm trụ VinFast/V-Green còn trống, đúng loại cổng/dòng xe ──> 4. Soạn chỉ dẫn gửi tài xế ──> 5. Gọi cứu hộ pin di động nếu cần

Bước nào tốn thời gian/lỗi nhất? Bước 3–4: tra trụ và soạn chỉ dẫn (⏱ ~10 phút/lượt trong tổng ~15 phút; ước lượng từ ví dụ lab, chưa xác thực).
AI có thể nhảy vào hỗ trợ ở bước nào? Bước 3–4: lấy dữ liệu xe và trạm, soạn bản nháp; dispatcher duyệt rồi mới gửi. AI không tự gửi, không tự điều cứu hộ.

Đo thành công bằng gì (Metric có số)?
  Thời gian xử lý ~15 phút ──> dưới 3 phút/lượt; chọn đúng trụ và đúng loại cổng ≥ 98%.

Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM  [ ] Agent
```

### Quick Problem Card #4 – Hùng

```
QUICK PROBLEM CARD #4 (Hùng)

Bài toán (1 câu): Tự động tra cứu vị trí, trạm sạc trống và soạn chỉ dẫn khi tài xế Xanh SM báo hết pin, để điều phối viên chỉ cần duyệt.
Công ty thành viên: [ ] VinFast  [x] Xanh SM  [ ] Vinhomes  [ ] Vinmec  [ ] Khác

Ai đang đau (Actor)? Điều phối viên Trung tâm Điều vận; tài xế chờ lâu, giảm khả năng quay lại đón khách.

Workflow thủ công hiện tại (3-5 bước):
  1. Nhận cuộc gọi sự cố ──> 2. Tra GPS xe trên bản đồ nội bộ ──> 3. Mở dashboard tìm trạm VinFast còn trụ trống, đúng cổng theo dòng xe (VF5/VFe34/VF8/VF9) ──> 4. Soạn tin chỉ đường gửi qua App ──> 5. Pin quá thấp thì gọi xe cứu hộ

Bước nào tốn thời gian/lỗi nhất? Bước 3–4: tra trạm và soạn chỉ dẫn thủ công (⏱ ~10 phút/lượt, tổng ~15 phút; ước lượng, chưa có log hệ thống).
AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2–4: tra vị trí và trạm qua API, soạn nháp tin nhắn; pin dưới 5% thì kích hoạt cứu hộ thay vì chỉ đường xa.

Đo thành công bằng gì (Metric có số)?
  Thời gian xử lý ~15 phút ──> dưới 3–5 phút/lượt. Ước tính 150–300 sự cố/ngày toàn quốc (từ quy mô 30.000+ xe, thị phần 54,5%).

Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM  [ ] Agent
```

### Quick Problem Card #5 – An

```
QUICK PROBLEM CARD #5 (An)

Bài toán (1 câu): Phân loại phản ánh của cư dân Vinhomes và chuyển đúng bộ phận phụ trách, thay cho đọc và phân loại thủ công.
Công ty thành viên: [ ] VinFast  [ ] Xanh SM  [x] Vinhomes  [ ] Vinmec  [ ] Khác

Ai đang đau (Actor)? Nhân viên chăm sóc khách hàng Vinhomes; cư dân chờ xử lý.

Workflow thủ công hiện tại (3-5 bước):
  1. Tiếp nhận phản ánh của cư dân ──> 2. Đọc hiểu nội dung ──> 3. Xác định loại vấn đề ──> 4. Chuyển đến bộ phận phụ trách

Bước nào tốn thời gian/lỗi nhất? Bước 2–4: đọc, phân loại, chuyển bộ phận; dễ phân loại sai hoặc chuyển nhầm (⏱ chưa có số).
AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2–4: LLM phân loại yêu cầu và chuyển tiếp tới bộ phận liên quan; nhân viên kiểm tra các trường hợp quan trọng.

Đo thành công bằng gì (Metric có số)?
  Chưa có số. Đề xuất đo: thời gian phân loại/phản ánh và tỷ lệ chuyển đúng bộ phận; chốt mục tiêu sau khi có baseline.

Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM  [ ] Agent
```

Ghi chú: Card #3 và #4 cùng một case (Xanh SM – xử lý sự cố pin thực địa). Card #1 đánh Rule vì mẫu không có ô riêng cho ML dự báo.

---

## Quyết định lựa chọn của nhóm

Nhóm chọn **Card #2 – Xanh SM (GSM): Chatbot CSKH trả lời câu hỏi ngoài kịch bản** (Khánh) để thực hiện Deep-Dive.

### Bảng điểm WOS của 5 card

| Card | Người | Case | WOS | Nhóm |
|------|-------|------|-----|------|
| #1 | Bảo | Vinhomes – Smart Urban Resource Optimization | 89 | High Opportunity |
| #2 | Khánh | Xanh SM – Chatbot CSKH ngoài kịch bản | 88 | High Opportunity |
| #5 | An | Vinhomes – Xử lý yêu cầu khách hàng | 87 | High Opportunity |
| #4 | Hùng | Xanh SM – Xử lý sự cố pin thực địa | 82 | High Opportunity |
| #3 | Đạt | Xanh SM – Xử lý sự cố pin thực địa | 80 | High Opportunity |

Điểm WOS là đánh giá sơ bộ của từng thành viên từ thông tin công khai và giả định, chưa qua ba Gate.

### Điểm của case được chọn

| Tiêu chí | Trọng số | Điểm | Đóng góp |
|----------|----------|------|----------|
| Business Impact | 25% | 4 | 20 |
| Pain Frequency & Scale | 20% | 5 | 20 |
| AI Advantage | 20% | 5 | 20 |
| Data Readiness | 15% | 4 | 12 |
| Implementation Feasibility | 10% | 3 | 6 |
| Measurability | 10% | 5 | 10 |
| **WOS** | 100% | | **88** |

### Lý do chọn

- AI Advantage 5/5: câu hỏi tự do ngoài kịch bản là điểm yếu của chatbot kịch bản cố định và là điểm mạnh của LLM. Đây là điểm cao nhất về tiêu chí này trong 5 card (card #1 đạt 4).
- Pain Frequency & Scale 5/5: thị trường taxi công nghệ Q1/2026 đạt 153,26 triệu chuyến, Xanh SM chiếm 54,51%, tương đương ~0,9 triệu chuyến/ngày (tự tính).
- Measurability 5/5: đo trực tiếp tỷ lệ tự giải quyết, thời gian phản hồi, CSAT.
- Data 4/5: đã có log chat, FAQ và chính sách làm nguồn trả lời.

### Lý do không chọn các card khác

- Card #1 (Smart Urban, 89): WOS cao hơn nhưng cần quyền truy cập hệ thống OT (SCADA/meter) và tách điện công cộng khỏi điện cư dân.
- Card #5 (Vinhomes CSKH): rủi ro sai thông tin về phí quản lý hoặc tranh chấp căn hộ có thể dẫn đến khiếu nại pháp lý; nên bắt đầu bằng rule-based router.
- Card #3, #4 (Xanh SM pin thực địa): baseline 15 phút chỉ là ước lượng từ ví dụ lab, cần xác thực tại hiện trường; sai trạm hoặc sai cổng có thể làm xe cạn pin giữa đường.

---

## Phase 3 – DEEP-DIVE (Nhóm)

### 3.1. Current-State Workflow

Quy trình hiện tại khi khách hỏi ngoài kịch bản của chatbot Xanh SM:

```
┌──────────────┐   ┌──────────────┐   ┌──────────────┐   ┌──────────────┐
│ Bước 1       │   │ Bước 2       │   │ Bước 3       │   │ Bước 4       │
│ Khách hỏi    │   │ Chatbot kịch │   │ Chuyển sang  │   │ Nhân viên    │
│ (chat/1555)  │──>│ bản không    │──>│ nhân viên    │──>│ tra chính    │
│              │   │ khớp câu hỏi │   │ 🔴           │   │ sách, trả lời│
│ Ai: Khách    │   │ Ai: Chatbot  │   │ Ai: Hệ thống │   │ Ai: Nhân viên│
│ ⏱ chưa có số │   │ ⏱ chưa có số │   │ ⏱ chưa có số │   │ ⏱ chưa có số │
│              │   │              │   │ (khách chờ)  │   │              │
└──────────────┘   └──────────────┘   └──────────────┘   └──────────────┘
```

🔴 = Bottleneck dự kiến: chuyển nhân viên và thời gian khách chờ. Chưa có log hệ thống để xác nhận.

Chưa có baseline: số lượt liên hệ CSKH/ngày, tỷ lệ chatbot hiện tại tự giải quyết, thời gian chờ chuyển nhân viên. Phải thu thập ở Phase 4.

### 3.2. Problem Statement (6-field) — Vin Smart Future Standard

| Field | Nội dung |
|-------|----------|
| 1. Actor / Operator | Khách hàng Xanh SM (GSM) và nhân viên CSKH/tổng đài 1555. |
| 2. Current Workflow | Khách hỏi qua chatbot kịch bản cố định. Câu hỏi ngoài kịch bản phải chuyển nhân viên, khách chờ, nhân viên tra chính sách rồi trả lời. 4 bước, thủ công ở bước 3–4. |
| 3. Bottleneck | Bước 3–4: câu hỏi ngoài kịch bản đều đổ về nhân viên, khách chờ lâu. Đây là giả thuyết, chưa có số liệu hệ thống. |
| 4. Business Impact | Quy mô tham chiếu: 153,26 triệu chuyến/quý toàn ngành, Xanh SM chiếm 54,51% ⇒ ~83 triệu chuyến/quý (~0,9 triệu chuyến/ngày, tự tính). Giả định 1% chuyến phát sinh liên hệ CSKH ⇒ ~9.000 lượt/ngày (chưa kiểm chứng). Lợi ích chủ yếu là giảm tải tổng đài và giảm chi phí CSKH; kênh 1555 tính phí khách 1.000đ/phút nên chatbot tốt cũng giúp khách đỡ tốn chi phí. |
| 5. Success Metric | 1. Tỷ lệ tự giải quyết (không chuyển người).<br>2. Thời gian phản hồi.<br>3. CSAT.<br>4. Tỷ lệ trả lời sai chính sách (mục tiêu 0 cho case hoàn tiền, an toàn).<br>Mục tiêu số chốt sau khi có baseline Phase 4. |
| 6. Operational Boundary | AI được phép: trả lời dựa trên FAQ/chính sách chính thức, có trích nguồn. CẤM: bịa chính sách; hứa hoặc tự quyết hoàn tiền; tự xử lý case nhạy cảm (hoàn tiền, an toàn). Khi không chắc hoặc gặp case nhạy cảm, bắt buộc chuyển nhân viên kèm tóm tắt hội thoại. |

### 3.3. Future-State Flow & AI Fit

**AI Fit:** chọn **LLM Feature** trả lời dựa trên FAQ/chính sách chính thức (truy xuất tài liệu trước khi trả lời). Không chọn Agent tự trị vì bài toán chỉ cần trả lời và chuyển người, không cần tự thực hiện hành động; chatbot nói chuyện trực tiếp với khách nên rủi ro bịa chính sách cao, cần ranh giới chặt.

Quy trình tương lai:

```
┌──────────────┐   ┌──────────────┐   ┌──────────────┐   ┌──────────────┐
│ Bước 1       │   │ Bước 2       │   │ Bước 3       │   │ Bước 4       │
│ Khách hỏi    │   │ 🔵 AI truy   │   │ 🔵 AI phân   │   │ 🟢 Trả lời   │
│ (chat/1555)  │──>│ xuất FAQ,    │──>│ loại: nhạy   │──>│ khách kèm    │
│              │   │ chính sách   │   │ cảm/không    │   │ nguồn        │
│              │   │ liên quan    │   │ chắc?        │   │              │
└──────────────┘   └──────────────┘   └──────────────┘   └──────────────┘
                                             │
                                             ▼
                                      ↩️ Fallback:
                                      Nhạy cảm (hoàn tiền, an toàn) hoặc
                                      không chắc ──> chuyển nhân viên kèm
                                      tóm tắt, nhân viên trả lời như cũ.
```

### 3.4. Rủi ro và điểm cần xác thực

- Chưa có baseline thật: số lượt liên hệ CSKH/ngày, tỷ lệ chatbot hiện tại tự giải quyết. Các số trên là giả định.
- Hallucination: chatbot nói trực tiếp với khách, nếu bịa chính sách hoặc hứa hoàn tiền sai sẽ gây khiếu nại.
- Implementation Feasibility 3/5: phải tích hợp hệ thống ticket, dữ liệu chuyến đi, cơ chế chuyển nhân viên.
- Chi phí LLM theo từng hội thoại ở quy mô lớn cần tính kỹ.
- Business Impact 4/5: lợi ích chủ yếu là giảm chi phí CSKH, không trực tiếp tăng doanh thu.

### Nguồn

- LSVN (23/04/2026), dẫn Mordor Intelligence: Green SM chiếm 54,51% thị phần taxi công nghệ Q1/2026; toàn ngành 153,26 triệu chuyến, GMV 580,71 triệu USD. https://lsvn.vn/green-sm-chiem-gan-55-thi-phan-taxi-cong-nghe-viet-nam-trong-quy-i-2026-a172151.html
- Green SM: chuyển tổng đài CSKH sang đầu số 1555 (24/7, 1.000đ/phút, từ 14/04/2026). https://www.greensm.com/vn-vi/news/green-sm-chinh-thuc-chuyen-doi-tong-dai-cham-soc-khach-hang-sang-dau-so-1555
- The EV Report (15/04/2024): GSM phục vụ hơn 50 triệu khách trong năm đầu. https://theevreport.com/gsm-hits-50-million-customers
- "~0,9 triệu chuyến/ngày" là số tự tính từ nguồn trên; "1% chuyến phát sinh liên hệ CSKH" là giả định, chưa kiểm chứng.
