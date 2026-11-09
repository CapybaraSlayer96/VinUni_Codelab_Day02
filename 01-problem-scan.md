# 01 — Problem Scan & Quick Cards

**Nhóm:** _(điền tên nhóm)_
**Thành viên thực hiện phần này:** Hùng — hungvu.education@gmail.com

---

## Phase 1 — SCAN (Cá nhân)

Quét qua vận hành các công ty thành viên Vingroup bằng 4 Lenses: **Lặp lại / Tốn thời gian / AI-upgrade / Pain từ người khác**.

| # | Subsidiary | Lens | Mô tả ngắn bài toán |
|---|------------|------|---------------------|
| 1 | **Xanh SM (GSM)** | Lặp lại | Điều phối viên xử lý thủ công sự cố pin/hỏng xe thực địa: tra GPS, tra trạm sạc trống, soạn tin chỉ dẫn — lặp lại hàng chục lần/ngày theo cùng một khuôn mẫu. |
| 2 | **VinFast** | AI có thể tốt hơn | Khách hàng mô tả lỗi xe bằng tiếng Việt tự nhiên (vd "xe kêu cụp cụp ở bánh trước khi qua gờ giảm tốc") nhưng tổng đài phải tự diễn giải sang mã lỗi kỹ thuật — chậm, tùy thuộc kinh nghiệm cá nhân từng nhân viên. |
| 3 | **Vinhomes** | Lặp lại | Phản ánh của cư dân qua App Vinhomes Resident (mất nước, hỏng đèn, ồn ào...) được phân loại và điều hướng thủ công đến đúng ban quản lý từng tòa — tốn thời gian, dễ gửi sai bộ phận. |
| 4 | **Vinmec** | Tốn thời gian | Bác sĩ mất 20-30 phút/bệnh nhân để soạn tóm tắt hồ sơ xuất viện (discharge summary) từ bệnh án điện tử, xét nghiệm và ghi chú — lặp lại với mọi ca xuất viện. |
| 5 | **Vinpearl** | Pain từ người khác | Quản lý khách sạn phải tự đọc hàng trăm review trên Booking/Agoda/Google Map để lọc ra phàn nàn khẩn cấp (phòng bẩn, thái độ nhân viên) — dễ bỏ lỡ phản hồi cần xử lý gấp. |

> *Ghi chú phương pháp:* 5 bài toán trên được quét theo đúng 4 lens trong bài giảng Day 02 (xem `../Day-02-Xac-dinh-bai-toan-cho-AI.md`), tham khảo thêm `minilab/VinUni_Codelab_Day02/03-inspiration-kit.md`. Chưa đề xuất giải pháp ở bước này — đúng nguyên tắc "Problem First, AI Second".

---

## Phase 2 — QUICK-ASSESS (Cá nhân)

Chọn **top 3** từ danh sách Scan, điền Quick Problem Card cho mỗi bài.

### Quick Problem Card #1 — Xanh SM: Xử lý sự cố pin thực địa ⭐ (khuyến nghị Deep-Dive)

```
Bài toán (1 câu): Tài xế Xanh SM báo hết/cạn pin giữa đường cần điều phối
viên tra cứu vị trí + trạm sạc trống + soạn chỉ dẫn, hoàn toàn thủ công.
Công ty thành viên: [x] Xanh SM (GSM)

Ai đang đau (Actor)? Điều phối viên (quá tải), Tài xế (chờ đợi)

Workflow thủ công hiện tại (5 bước):
  1. Nhận gọi báo sự cố ──> 2. Tra GPS xe ──> 3. Tra trạm sạc VinFast
  trống phù hợp dòng xe ──> 4. Soạn tin chỉ dẫn gửi tài xế ──> 5. Gọi
  cứu hộ nếu cần

Bước nào tốn thời gian/lỗi nhất? Bước 3-4 (⏱ ~10 phút/lượt — ƯỚC LƯỢNG
CHƯA KIỂM CHỨNG, cần phỏng vấn điều phối viên thật trước khi đưa vào
Problem Statement chính thức)
AI có thể nhảy vào hỗ trợ ở bước nào? Bước 3-4 (tự động tra trạm +
draft tin chỉ dẫn)

Đo thành công bằng gì (Metric có số)?
  Giảm thời gian xử lý sự cố từ 15 phút ──> dưới 3 phút.
  (⚠️ Số "15 phút" cũng là ước lượng chưa kiểm chứng — cần baseline
  thật từ log/ca trực trước khi tính vào Gate G2.)

Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM  [ ] Agent

Đánh giá (CFO/Vận hành review): workflow 5 bước cụ thể, boundary tự
nhiên — AI chỉ gợi ý/soạn draft, con người (điều phối viên) vẫn quyết
định cuối. Rủi ro thấp, không nhạy cảm pháp lý/y tế → khả năng đạt
"Go" cao nhất trong 3 card. ĐỀ XUẤT chọn bài này cho Deep-Dive.
```

### Quick Problem Card #2 — Vinhomes: Phân loại phản ánh cư dân ❌ (không đề xuất Deep-Dive)

```
Bài toán (1 câu): Phản ánh cư dân qua App (mất nước, hỏng đèn, ồn ào...)
hiện được đọc và điều hướng thủ công đến đúng ban quản lý tòa nhà.
Công ty thành viên: [x] Vinhomes

Ai đang đau (Actor)? Nhân viên tiếp nhận phản ánh, cư dân (chờ xử lý)

Workflow thủ công hiện tại (4 bước):
  1. Cư dân gửi phản ánh trên App ──> 2. Nhân viên đọc, phân loại
  bằng tay ──> 3. Chuyển đến đúng ban quản lý/tòa ──> 4. Ban quản lý
  xử lý & phản hồi

Bước nào tốn thời gian/lỗi nhất? Bước 2 (phân loại sai gây chuyển
nhầm bộ phận, mất thêm vòng chuyển tiếp)

AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2 (tự động phân loại +
gắn đúng tòa/ban quản lý)

Đo thành công bằng gì (Metric có số)?
  [CHƯA CÓ SỐ THẬT] Bản gốc ghi "giảm tỷ lệ chuyển nhầm từ ~X% xuống
  <5%" — đây là lỗi "No baseline" (Anti-pattern trong bài giảng):
  không có dữ liệu thật, chỉ là khung mẫu chưa điền số.

Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM  [ ] Agent

Operational Boundary: [CHƯA CÓ — cần bổ sung]. Đây là bài toán có
yếu tố an toàn vận hành (mất nước/hỏng đèn có thể là sự cố khẩn cấp),
nhưng card chưa trả lời: AI sai phân loại thì hậu quả gì? Khi nào
bắt buộc người duyệt trước khi chuyển đi?

Đánh giá (CFO/Vận hành review): lỗi nặng nhất trong 3 card — vừa
thiếu metric thật, vừa thiếu hoàn toàn Operational Boundary cho một
bài toán có rủi ro an toàn. Vi phạm lưu ý "ranh giới phải cực kỳ
nghiêm ngặt" trong inspiration-kit. KHÔNG đề xuất chọn cho Deep-Dive
trừ khi nhóm bổ sung đầy đủ số liệu + boundary trước.
```

### Quick Problem Card #3 — Vinmec: Tóm tắt hồ sơ xuất viện ⚠️ (cần siết ranh giới trước khi dùng)

```
Bài toán (1 câu): Bác sĩ mất 20-30 phút/bệnh nhân để soạn tóm tắt hồ sơ
xuất viện bằng ngôn ngữ dễ hiểu từ bệnh án điện tử, xét nghiệm, ghi chú.
Công ty thành viên: [x] Vinmec

Ai đang đau (Actor)? Bác sĩ (quá tải cuối ca), Bệnh nhân (chờ xuất viện)

Workflow thủ công hiện tại (4 bước):
  1. Tổng hợp dữ liệu bệnh án + xét nghiệm ──> 2. Đọc lại ghi chú điều
  trị ──> 3. Viết tóm tắt xuất viện bằng ngôn ngữ cho bệnh nhân ──>
  4. Bác sĩ ký duyệt

Bước nào tốn thời gian/lỗi nhất? Bước 1+3 (tổng hợp dữ liệu rải rác +
viết lại dễ hiểu, ⏱ ~20-30 phút — ƯỚC LƯỢNG CHƯA KIỂM CHỨNG, cần
phỏng vấn bác sĩ thật, không chỉ suy luận)

AI có thể nhảy vào hỗ trợ ở bước nào? Bước 1-3 (tự động kéo dữ liệu +
draft tóm tắt, bác sĩ chỉ review/sửa)

Đo thành công bằng gì (Metric có số)?
  Giảm thời gian soạn tóm tắt từ 25 phút ──> dưới 8 phút/bệnh nhân,
  không giảm độ chính xác lâm sàng (bác sĩ vẫn ký duyệt 100%).
  (⚠️ Số "25 phút" cũng chưa kiểm chứng.)

Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM  [ ] Agent

Operational Boundary (cần siết chặt hơn): "Bác sĩ ký duyệt 100%" chỉ
là bước có sẵn trong workflow gốc, CHƯA phải ranh giới mới được thiết
kế riêng cho rủi ro AI. Với bài toán y tế, cần bổ sung rõ: (1) AI sai/
thiếu thông tin lâm sàng trong bản tóm tắt thì hậu quả gì, (2) ai chịu
trách nhiệm pháp lý nếu bác sĩ ký duyệt một bản tóm tắt do AI soạn có
sai sót, (3) AI tuyệt đối không được tự suy diễn chỉ số lâm sàng không
có trong hồ sơ gốc. Theo khung 5-câu đánh giá AI-fit, câu "hậu quả khi
AI sai có nằm trong tầm kiểm soát?" hiện CHƯA trả lời được rõ ràng.

Đánh giá (CFO/Vận hành review): rủi ro cao hơn Card #1 do dữ liệu
bệnh án nhạy cảm + hậu quả sai có thể ảnh hưởng sức khỏe. Nếu nhóm
vẫn muốn chọn bài này, Go/Not-Go khó đạt "Go" ngay ở vòng đầu — nhiều
khả năng rơi vào "Not Yet" cho đến khi boundary được siết chặt.
```

---

## Ghi chú cho buổi họp nhóm

Ba card trên mang đến buổi họp để nhóm phản biện chéo và chốt 1 bài toán chung (Phase 3 — Deep-Dive).

**Đã qua 1 vòng phản biện độc lập (vai CFO/Vận hành khắt khe) trước khi mang ra họp nhóm:**

| Card | Verdict | Lý do |
|---|---|---|
| **#1 — Xanh SM (pin)** ⭐ | **Đề xuất chọn** | Workflow cụ thể, boundary tự nhiên (AI chỉ gợi ý, người quyết định), rủi ro thấp, không nhạy cảm pháp lý/y tế → khả năng đạt "Go" cao nhất |
| **#2 — Vinhomes (phản ánh)** ❌ | **Không đề xuất** | Metric "~X%" là lỗi "No baseline" lộ liễu; hoàn toàn thiếu Operational Boundary cho bài toán có yếu tố an toàn vận hành |
| **#3 — Vinmec (hồ sơ xuất viện)** ⚠️ | **Cân nhắc nếu siết chặt boundary** | Có HITL thật (bác sĩ ký duyệt) nhưng ranh giới mới ở mức "đủ" chứ chưa "cực kỳ nghiêm ngặt" như yêu cầu cho bài toán y tế |

**Phát hiện chung quan trọng nhất:** toàn bộ số liệu trong cả 5 bài toán Phase 1 và 3 card (10 phút, 15 phút, 20-30 phút, 25 phút...) đều là **ước lượng hợp lý, chưa phải baseline đo được thật** — đã đánh dấu rõ trong từng card. Trước khi nộp bản chính thức (Gate G2 — "metric có số"), nhóm cần thay các số này bằng dữ liệu thật (phỏng vấn người thực hiện quy trình/log hệ thống) hoặc ghi rõ "ước lượng chưa kiểm chứng" nếu chưa kịp thu thập.

Quyết định cuối cùng chọn bài toán nào vẫn cần cả nhóm đồng thuận — ưu tiên bài toán mà ít nhất 1 thành viên hiểu rõ quy trình thật (theo lưu ý trong `03-inspiration-kit.md`).
