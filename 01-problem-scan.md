# 01 — Problem Scan & Quick Cards

**Tên nhóm:** The Hive

| Họ và tên         | Email                        |
| ----------------- | ---------------------------- |
| Triệu Quốc Khánh  | trieukhanh25032005@gmail.com |
| Vũ Quốc Đạt       | dvuquoc60@gmail.com          |
| Hoàng Đức An      | anduchoang2701@gmail.com     |
| Vũ Tiến Hùng      | hungvu.education@gmail.com   |
| Đoàn Xuân Duy Bảo | doanxuanduybao96@gmail.com   |

**Người thực hiện file này:** Triệu Quốc Khánh — trieukhanh25032005@gmail.com

---

# 🔍 Phase 1 — SCAN (Cá nhân)

**Phạm vi quét:** Xanh SM (GSM) — vận hành taxi điện.
**Lens sử dụng:** cả 4 lens (Lặp lại, Tốn thời gian, AI có thể tốt hơn, Pain từ người khác).

> Ghi chú: các con số thời gian/khối lượng trong bảng là **ước tính** để định cỡ bài toán, chưa phải số liệu thực tế của Xanh SM.

| #   | Subsidiary  | Lens               | Mô tả ngắn bài toán                                                                                                                                                               |
| --- | ----------- | ------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1   | **Xanh SM** | Lặp lại            | Khách để quên đồ trên xe: tổng đài viên phải tra lại chuyến, gọi tài xế, hẹn lịch trả đồ — xử lý thủ công khoảng 10–15 phút/ca (ước tính).                                        |
| 2   | **Xanh SM** | Tốn thời gian      | Phòng Chất lượng dịch vụ phải đọc và phân loại thủ công các đánh giá 1–2 sao (thái độ tài xế, xe bẩn, app lỗi, giá cước) để làm báo cáo tuần — mất nhiều giờ mỗi tuần (ước tính). |
| 3   | **Xanh SM** | AI có thể tốt hơn  | Chatbot CSKH hiện trả lời theo kịch bản cố định, phần lớn câu hỏi ngoài kịch bản phải chuyển sang nhân viên, khiến khách chờ lâu.                                                 |
| 4   | **Xanh SM** | AI có thể tốt hơn  | Tài xế không giao tiếp được với khách nước ngoài (hỏi đường, dặn điểm dừng, đồ thất lạc), dẫn tới đi sai điểm và bị đánh giá thấp.                                                |
| 5   | **Xanh SM** | Pain từ người khác | Tài xế phàn nàn phải xếp hàng chờ sạc do nhiều xe cùng dồn về một trạm giờ cao điểm — mất thời gian chạy xe, giảm thu nhập.                                                       |

---

# 🃏 Phase 2 — QUICK-ASSESS (Cá nhân)

**Top 3 bài toán được chọn từ SCAN:** #3 (Chatbot CSKH), #2 (Phân loại đánh giá thấp), #5 (Xếp hàng chờ sạc).
**Lý do chọn:** xếp hạng theo thang WOS của nhóm (Weighted Sum Model: Impact 25%, Pain 20%, AI Advantage 20%, Data 15%, Feasibility 10%, Measurability 10%) — #3 đạt 88, #2 đạt 77, #5 đạt 74; loại #4 (70) và #1 (59). Giữ #5 dù AI Advantage thấp để minh họa trường hợp Rule tốt hơn LLM.

> Ghi chú: các con số thời gian/khối lượng là **ước tính** để định cỡ bài toán, cần xác minh bằng dữ liệu vận hành thực tế.

## Quick Problem Card #1 — Chatbot CSKH trả lời câu hỏi ngoài kịch bản

| Trường                          | Nội dung                                                                                                                                                                                                                                      |
| ------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Bài toán (1 câu)**            | Chatbot CSKH hiện trả lời theo kịch bản cố định, câu hỏi ngoài kịch bản phải chuyển sang nhân viên nên khách chờ lâu và tổng đài quá tải bởi câu hỏi lặp lại.                                                                                 |
| **Công ty thành viên**          | Xanh SM                                                                                                                                                                                                                                       |
| **Actor (ai đang đau)**         | Khách hàng (chờ phản hồi), nhân viên CSKH (quá tải câu hỏi lặp lại)                                                                                                                                                                           |
| **Workflow hiện tại**           | 1. Khách nhắn chatbot trên app ──> 2. Chatbot so khớp câu hỏi với kịch bản/từ khóa ──> 3. Không khớp → chuyển vào hàng chờ nhân viên ──> 4. Nhân viên đọc lại hội thoại, tra chính sách/thông tin chuyến, trả lời thủ công ──> 5. Đóng ticket |
| **Bước tốn thời gian/lỗi nhất** | Bước 3–4: chờ nhân viên và xử lý thủ công — ⏱ ~5–15 phút chờ/lượt, ~5 phút xử lý/câu hỏi lặp lại (ước tính)                                                                                                                                   |
| **AI nhảy vào ở bước nào**      | Bước 2–4: LLM hiểu câu hỏi tự do → trả lời dựa trên tài liệu chính sách/FAQ chính thức và thông tin chuyến đi của chính khách → khi không chắc chắn hoặc thuộc nhóm nhạy cảm thì chuyển nhân viên kèm tóm tắt hội thoại.                      |
| **Ranh giới an toàn**           | Chỉ trả lời dựa trên chính sách chính thức, không tự bịa thông tin; không tự hứa/duyệt hoàn tiền, bồi thường; khiếu nại an toàn, tai nạn, quấy rối → chuyển người ngay; không tiết lộ thông tin cá nhân của tài xế hay khách khác.            |
| **Metric (có số)**              | Tỷ lệ chatbot tự giải quyết ≥ 60% câu hỏi (baseline cần đo); thời gian phản hồi câu hỏi thường gặp từ ~10 phút ──> dưới 1 phút; 100% case nhạy cảm được chuyển người; CSAT không giảm.                                                        |
| **Quick Architecture**          | [ ] No AI [ ] Rule **[x] LLM** [ ] Agent — câu hỏi tự do ngoài kịch bản là điểm yếu của rule, điểm mạnh của LLM; chatbot chỉ trả lời/chuyển tiếp, không tự thực hiện hành động nên chưa cần Agent.                                            |

## Quick Problem Card #2 — Phân loại đánh giá 1–2 sao

| Trường                          | Nội dung                                                                                                                                                                                                                                                                |
| ------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Bài toán (1 câu)**            | Phòng Chất lượng dịch vụ phải đọc và gắn nhãn thủ công từng đánh giá 1–2 sao để tìm nguyên nhân, làm báo cáo tuần chậm và dễ bỏ sót case nghiêm trọng.                                                                                                                  |
| **Công ty thành viên**          | Xanh SM                                                                                                                                                                                                                                                                 |
| **Actor (ai đang đau)**         | Chuyên viên Phòng Chất lượng dịch vụ; gián tiếp: khách hàng có khiếu nại nghiêm trọng bị xử lý chậm                                                                                                                                                                     |
| **Workflow hiện tại**           | 1. Xuất toàn bộ đánh giá 1–2 sao trong tuần ra Excel ──> 2. Đọc từng bình luận ──> 3. Gắn nhãn nguyên nhân thủ công (thái độ tài xế / xe bẩn / app lỗi / giá cước / an toàn) ──> 4. Tổng hợp bằng pivot, viết báo cáo ──> 5. Chuyển case nghiêm trọng cho bộ phận xử lý |
| **Bước tốn thời gian/lỗi nhất** | Bước 2–3: đọc và gắn nhãn — ⏱ ~1 phút/đánh giá × hàng nghìn đánh giá/tuần (ước tính)                                                                                                                                                                                    |
| **AI nhảy vào ở bước nào**      | Bước 2–3 và 5: LLM đọc bình luận → gắn nhãn nguyên nhân + trích câu bằng chứng → gắn cờ khẩn cấp cho case an toàn/quấy rối. Chuyên viên kiểm tra mẫu ngẫu nhiên và các case bị gắn cờ.                                                                                  |
| **Ranh giới an toàn**           | LLM chỉ phân loại, không tự phản hồi khách hay xử phạt tài xế; case an toàn luôn chuyển cho người xử lý.                                                                                                                                                                |
| **Metric (có số)**              | Giảm thời gian làm báo cáo tuần từ ~2 ngày công ──> dưới 2 giờ; độ chính xác gắn nhãn ≥ 90% khi kiểm tra mẫu; case an toàn được chuyển đi trong dưới 1 giờ.                                                                                                             |
| **Quick Architecture**          | [ ] No AI [ ] Rule **[x] LLM** [ ] Agent — văn bản tự do, nhiều cách diễn đạt, rule theo từ khóa sẽ bỏ sót; không cần nhiều bước hành động nên chưa cần Agent.                                                                                                          |

## Quick Problem Card #3 — Xếp hàng chờ sạc giờ cao điểm

| Trường                          | Nội dung                                                                                                                                                                                              |
| ------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Bài toán (1 câu)**            | Tài xế tự chọn trạm sạc theo thói quen nên nhiều xe dồn về cùng một trạm giờ cao điểm, phải xếp hàng chờ lâu, mất thời gian chạy xe.                                                                  |
| **Công ty thành viên**          | Xanh SM (phối hợp hạ tầng trạm sạc VinFast)                                                                                                                                                           |
| **Actor (ai đang đau)**         | Tài xế (mất thu nhập), điều phối viên (thiếu xe giờ cao điểm)                                                                                                                                         |
| **Workflow hiện tại**           | 1. Tài xế thấy pin thấp ──> 2. Tự chọn trạm quen/gần nhất ──> 3. Đến nơi mới biết đang có hàng chờ ──> 4. Chờ hoặc chạy sang trạm khác                                                                |
| **Bước tốn thời gian/lỗi nhất** | Bước 3–4: chờ sạc — ⏱ ~20–40 phút/lượt giờ cao điểm (ước tính)                                                                                                                                        |
| **AI nhảy vào ở bước nào**      | Bước 2: hệ thống gợi ý trạm dựa trên số cổng trống theo thời gian thực, số xe đang trên đường tới, khoảng cách và % pin còn lại.                                                                      |
| **Ranh giới an toàn**           | Pin dưới 5% không gợi ý trạm xa hơn 5km mà chuyển sang điều xe sạc lưu động (khớp với rule của prompt prototype).                                                                                     |
| **Metric (có số)**              | Giảm thời gian chờ sạc trung bình giờ cao điểm từ ~30 phút ──> dưới 10 phút; giảm số trạm quá tải (hàng chờ > 3 xe) xuống dưới 10%.                                                                   |
| **Quick Architecture**          | [ ] No AI **[x] Rule** [ ] LLM [ ] Agent — dữ liệu đầu vào có cấu trúc (số cổng, khoảng cách, % pin), cần kết quả chính xác và nhất quán, độ trễ thấp; thuật toán tối ưu/rule rẻ và đáng tin hơn LLM. |
