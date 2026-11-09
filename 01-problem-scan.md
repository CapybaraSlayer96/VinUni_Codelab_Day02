# 01 — Problem Scan & Quick Cards

**Nhóm:** The Hive

**Thành viên:**
- Đoàn Xuân Duy Bảo — doanxuanduybao96@gmail.com
- Triệu Quốc Khánh — trieukhanh25032005@gmail.com
- Vũ Quốc Đạt — dvuquoc60@gmail.com
- Vũ Tiến Hùng — hungvu.education@gmail.com
- Hoàng Đức An — anduchoang2701@gmail.com

**Thành viên thực hiện phần này:** Vũ Tiến Hùng — hungvu.education@gmail.com

---

## Phase 1 — SCAN (Cá nhân)

Quét qua vận hành các công ty thành viên Vingroup bằng 4 Lenses: **Lặp lại / Tốn thời gian / AI-upgrade / Pain từ người khác**.

Theo yêu cầu của giảng viên: mọi ý tưởng phải được quy về số liệu để đánh giá và chọn. Nhóm không có dữ liệu nội bộ Vingroup nên dùng **benchmark công khai** (báo chí, số liệu kinh doanh đã công bố, khảo sát lương thị trường) làm cơ sở ước tính — số liệu dưới đây là **ước tính bậc độ lớn (order-of-magnitude), có nguồn trích dẫn, không phải số liệu nội bộ chính thức**.

| # | Subsidiary | Lens | Mô tả ngắn bài toán | Quy mô tham chiếu (có nguồn) | Giả định tính toán | Tác động dự báo nếu áp dụng AI |
|---|---|---|---|---|---|---|
| 1 | **Xanh SM (GSM)** | Lặp lại | Điều phối viên xử lý thủ công sự cố pin/hỏng xe thực địa: tra GPS, tra trạm sạc trống, soạn tin chỉ dẫn — lặp lại hàng chục lần/ngày theo cùng một khuôn mẫu. | >30.000 xe taxi điện toàn quốc, chiếm **54,5% thị phần taxi công nghệ** Q1/2026 (Mordor Intelligence, qua [greensm.com](https://www.greensm.com/vn-vi/news/chiem-gan-45-thi-phan-xanh-sm-ngay-cang-vuot-xa-cac-doi-thu-tren-thi-truong)) | ~0,5-1% ca vận hành/ngày phát sinh sự cố pin cần điều phối hỗ trợ → **150-300 sự cố/ngày toàn quốc**; mỗi sự cố tốn ~10 phút thủ công ở bước nghẽn (bước 3-4) | **Tiết kiệm thời gian:** ~25-50 giờ công điều phối/ngày toàn quốc. **Tiết kiệm chi phí:** lương điều phối viên benchmark 7-15 triệu VNĐ/tháng ([JobsGO](https://jobsgo.vn/viec-lam-dieu-phoi-vien-tai-ho-chi-minh.html), trung vị ~10tr/tháng ≈ 45.000đ/giờ công) → giảm 80% thời gian bước nghẽn bằng AI tiết kiệm ước tính **~35-70 triệu VNĐ/tháng** toàn quốc. **Giảm rủi ro:** giảm nguy cơ xe cạn pin giữa đường gây tắc nghẽn giao thông + giảm tỷ lệ hủy chuyến do tài xế chờ lâu → bảo toàn doanh thu cuốc xe. |
| 2 | **VinFast** | AI có thể tốt hơn | Khách hàng mô tả lỗi xe bằng tiếng Việt tự nhiên nhưng tổng đài phải tự diễn giải sang mã lỗi kỹ thuật — chậm, tùy kinh nghiệm cá nhân từng nhân viên. | ~154.073 xe điện VinFast bán tại Việt Nam trong 8 tháng đầu 2026 ([Tiền Phong](https://tienphong.vn/vinfast-ban-20161-xe-o-to-dien-tai-viet-nam-trong-thang-82026-post1875712.tpo)) — dùng làm quy mô đội xe đang lưu hành tham chiếu | Giả định ~5% chủ xe gọi hotline hỗ trợ kỹ thuật/tháng → ~7.700 cuộc gọi/tháng ≈ **257 cuộc/ngày**; mỗi cuộc tốn thêm ~5 phút để tổng đài tự diễn giải mô tả lỗi sang mã kỹ thuật | **Tiết kiệm thời gian:** ~257 cuộc × 5 phút ≈ **21 giờ công tổng đài/ngày**. **Tiết kiệm chi phí:** cùng benchmark 45.000đ/giờ → giảm 80% thời gian này tiết kiệm ước tính **~28 triệu VNĐ/tháng**. **Giảm rủi ro:** giảm tỷ lệ điều sai loại hỗ trợ (vd gửi thợ cơ khí thường cho lỗi pin EV) → tăng CSAT, giảm chi phí điều xe cứu hộ sai lượt. |
| 3 | **Vinhomes** | Lặp lại | Phản ánh của cư dân qua App Vinhomes Resident (mất nước, hỏng đèn, ồn ào...) được phân loại và điều hướng thủ công đến đúng ban quản lý từng tòa — tốn thời gian, dễ gửi sai bộ phận. | Vinhomes Ocean Park 1 (1 trong nhiều khu đô thị Vinhomes toàn quốc): **~60.000 cư dân** hiện tại ([LSVN](https://lsvn.vn/ocean-city-he-sinh-thai-nuoi-duong-cong-dong-tinh-hoa-dong-dieu-a168967.html)) ≈ ~17.100 hộ (giả định 3,5 người/hộ) | Giả định ~5% hộ gửi phản ánh/tháng → ~855 phản ánh/tháng ≈ **28 phản ánh/ngày** (chỉ tính riêng 1 khu đô thị); mỗi ca phân loại sai tốn thêm ~5 phút xử lý lại | **Tiết kiệm thời gian:** ~28 × 5 phút ≈ **2,3 giờ công/ngày/khu đô thị** — nhỏ ở quy mô 1 dự án nhưng **nhân rộng được** qua hàng chục khu đô thị Vinhomes toàn quốc. **Giảm rủi ro:** giảm nguy cơ sự cố khẩn cấp (mất nước, chập điện) bị trễ xử lý do chuyển nhầm bộ phận. |
| 4 | **Vinmec** | Tốn thời gian | Bác sĩ mất 20-30 phút/bệnh nhân để soạn tóm tắt hồ sơ xuất viện (discharge summary) từ bệnh án điện tử, xét nghiệm và ghi chú — lặp lại với mọi ca xuất viện. | Vinmec Central Park (1 trong nhiều cơ sở Vinmec toàn quốc): **178 giường bệnh** ([Hellobacsi](https://hellobacsi.com/care/benh-vien/benh-vien-da-khoa-quoc-te-vinmec-central-park-1/)) | Giả định công suất sử dụng ~75%, thời gian nằm viện trung bình ~4,5 ngày → ~178×0,75/4,5 ≈ **30 ca xuất viện/ngày** (1 cơ sở); mỗi ca tốn ~25 phút soạn tóm tắt | **Tiết kiệm thời gian:** 30 ca × 17 phút (giảm từ 25→8 phút) ≈ **8,5 giờ công bác sĩ/ngày/cơ sở** — quy đổi gián tiếp thành tăng công suất khám/điều trị (không phải tiết kiệm tiền công trực tiếp). **Giảm rủi ro:** KHÔNG áp dụng theo hướng tích cực — đây là bài toán rủi ro cao (xem Card #3), cần boundary chặt vì AI sai ảnh hưởng sức khỏe. |
| 5 | **Vinpearl** | Pain từ người khác | Quản lý khách sạn phải tự đọc hàng trăm review trên Booking/Agoda/Google Map để lọc ra phàn nàn khẩn cấp — dễ bỏ lỡ phản hồi cần xử lý gấp. | **~18.500 phòng/villa** tại 45 cơ sở toàn quốc ([VTC News](https://www.vtcnews.vn/vinpearl-duoc-vinh-danh-voi-13-giai-thuong-du-lich-vietnam-travel-awards-ar864799.html)) | Giả định công suất phòng ~60% → ~11.100 phòng có khách/đêm; ước tính ~500 review mới/ngày toàn hệ thống cần đọc sàng lọc, mỗi review mất ~1 phút đọc | **Tiết kiệm thời gian:** giảm 80% thời gian đọc sàng lọc ≈ **~6,7 giờ công/ngày toàn hệ thống**, quy đổi tiền công benchmark ≈ **~9 triệu VNĐ/tháng** (giá trị trực tiếp nhỏ). **Giảm rủi ro (giá trị chính):** giảm nguy cơ bỏ lỡ phàn nàn khẩn cấp → giảm rủi ro mất khách/giảm rating → bảo toàn doanh thu & danh tiếng thương hiệu (chưa định lượng được phần này). |

> *Ghi chú phương pháp:* 5 bài toán trên được quét theo đúng 4 lens trong bài giảng Day 02 (xem `../Day-02-Xac-dinh-bai-toan-cho-AI.md`), tham khảo thêm `minilab/VinUni_Codelab_Day02/03-inspiration-kit.md`. Chưa đề xuất giải pháp ở bước này — đúng nguyên tắc "Problem First, AI Second".
>
> ⚠️ **Lưu ý về độ tin cậy số liệu (xếp theo độ tin cậy giảm dần):**
> 1. **Xanh SM** — benchmark mạnh nhất: cả quy mô đội xe và thị phần đều có nguồn số liệu thị trường rõ ràng, gần với bài toán (vận hành đội xe thực địa).
> 2. **VinFast** — quy mô đội xe có nguồn số liệu bán hàng chính thức rõ ràng, nhưng **tỷ lệ gọi hotline/tháng (~5%) là giả định không có nguồn**, cần VinFast xác nhận.
> 3. **Vinpearl** — quy mô phòng có nguồn, nhưng **tỷ lệ review/ngày và công suất phòng là giả định**.
> 4. **Vinmec** — chỉ dùng được số liệu của 1 cơ sở (Central Park); Vinmec Times City (nơi bài toán giả định xảy ra) có số liệu không thống nhất giữa các nguồn, nên quy mô ở đây chỉ mang tính minh họa thay thế.
> 5. **Vinhomes** — quy mô dân số có nguồn (báo chí) nhưng dao động lớn giữa các bài (45.000-60.000-200.000 tùy thời điểm/nguồn), và tỷ lệ gửi phản ánh/tháng hoàn toàn là giả định.
>
> Tất cả **giả định tính toán** (tỷ lệ sự cố/ngày, tỷ lệ gọi hotline, tỷ lệ phản ánh, công suất phòng...) đều **không có nguồn xác thực** — chỉ là ước lượng hợp lý để có cơ sở so sánh. Trước khi đưa vào Problem Statement chính thức, bài toán được chọn cần được doanh nghiệp xác nhận lại số liệu thật (xem Phase 4 — Validation).
>
> Bài toán #1 (Xanh SM) có **benchmark tin cậy nhất và gần nhất với bản chất bài toán vận hành thực địa**, nên được chọn làm Deep-Dive (khớp với kết luận ở Phase 2 bên dưới) — không phải vì các bài khác không đáng giá, mà vì đây là bài có nền tảng số liệu vững nhất để tiếp tục phân tích sâu trong thời gian giới hạn của buổi lab.

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

Bước nào tốn thời gian/lỗi nhất? Bước 3-4 (⏱ ~10 phút/lượt — ước lượng
thao tác, CHƯA có log thật; nhưng QUY MÔ đã có benchmark: ~150-300 sự
cố/ngày toàn quốc, dựa trên >30.000 xe GSM × 0,5-1% tỷ lệ sự cố/ngày,
nguồn Mordor Intelligence qua greensm.com — xem Phase 1)
AI có thể nhảy vào hỗ trợ ở bước nào? Bước 3-4 (tự động tra trạm +
draft tin chỉ dẫn)

Đo thành công bằng gì (Metric có số)?
  Giảm thời gian xử lý sự cố từ 15 phút ──> dưới 3 phút/lượt.
  Quy mô tác động: ~25-50 giờ công điều phối/ngày toàn quốc được giải
  phóng ──> tiết kiệm ước tính ~35-70 triệu VNĐ/tháng (benchmark lương
  điều phối 10tr/tháng, JobsGO) + giảm rủi ro xe cạn pin giữa đường.
  (⚠️ Số phút/lượt vẫn là ước lượng thao tác chưa kiểm chứng — cần
  baseline thật từ log/ca trực trước khi tính vào Gate G2; nhưng quy
  mô sự cố/ngày đã có cơ sở benchmark, không còn là số bịa hoàn toàn.)

Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM  [ ] Agent

Đánh giá (CFO/Vận hành review): workflow 5 bước cụ thể, boundary tự
nhiên — AI chỉ gợi ý/soạn draft, con người (điều phối viên) vẫn quyết
định cuối. Rủi ro thấp, không nhạy cảm pháp lý/y tế, và là bài có
benchmark quy mô tin cậy nhất trong 5 bài Scan → khả năng đạt "Go"
cao nhất trong 3 card. ĐỀ XUẤT chọn bài này cho Deep-Dive.
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
  Cập nhật sau khi có benchmark (xem Phase 1): Vinhomes Ocean Park 1
  ~60.000 cư dân (~17.100 hộ) ──> giả định ~5% hộ gửi phản ánh/tháng
  ≈ 28 phản ánh/ngày (1 khu đô thị). Target: giảm tỷ lệ chuyển nhầm
  bộ phận xuống <5%, tiết kiệm ~2,3 giờ công/ngày/khu đô thị.
  (⚠️ Quy mô dân số có nguồn báo chí, nhưng tỷ lệ "5% hộ gửi phản ánh/
  tháng" và target "<5%" vẫn là GIẢ ĐỊNH không có nguồn — khác với
  việc hoàn toàn không có số như bản gốc, nhưng vẫn chưa phải baseline
  thật. Quy mô này cũng chỉ 1 khu đô thị trong khi Vinhomes có hàng
  chục khu toàn quốc — chưa tổng hợp được quy mô toàn công ty.)

Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM  [ ] Agent

Operational Boundary: [CHƯA CÓ — cần bổ sung]. Đây là bài toán có
yếu tố an toàn vận hành (mất nước/hỏng đèn có thể là sự cố khẩn cấp),
nhưng card chưa trả lời: AI sai phân loại thì hậu quả gì? Khi nào
bắt buộc người duyệt trước khi chuyển đi?

Đánh giá (CFO/Vận hành review): dù đã có quy mô dân số tham chiếu,
card này vẫn **thiếu hoàn toàn Operational Boundary** cho một bài
toán có rủi ro an toàn — đây mới là lý do chính không đề xuất, không
còn phải vì "không có số" nữa. Vi phạm lưu ý "ranh giới phải cực kỳ
nghiêm ngặt" trong inspiration-kit. KHÔNG đề xuất chọn cho Deep-Dive
trừ khi nhóm bổ sung Operational Boundary đầy đủ trước.
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
viết lại dễ hiểu, ⏱ ~20-30 phút — ƯỚC LƯỢNG THAO TÁC CHƯA KIỂM CHỨNG,
cần phỏng vấn bác sĩ thật. Quy mô tham chiếu: Vinmec Central Park có
178 giường bệnh (nguồn Hellobacsi) ──> giả định công suất 75%, thời
gian nằm viện TB 4,5 ngày ≈ ~30 ca xuất viện/ngày/cơ sở — xem Phase 1)

AI có thể nhảy vào hỗ trợ ở bước nào? Bước 1-3 (tự động kéo dữ liệu +
draft tóm tắt, bác sĩ chỉ review/sửa)

Đo thành công bằng gì (Metric có số)?
  Giảm thời gian soạn tóm tắt từ 25 phút ──> dưới 8 phút/bệnh nhân,
  không giảm độ chính xác lâm sàng (bác sĩ vẫn ký duyệt 100%).
  Quy mô tác động: ~30 ca/ngày × 17 phút tiết kiệm ≈ 8,5 giờ công bác
  sĩ/ngày/cơ sở (1 trong nhiều cơ sở Vinmec toàn quốc).
  (⚠️ Số "25 phút" vẫn là ước lượng thao tác chưa kiểm chứng; số liệu
  178 giường là của Vinmec Central Park, không phải cơ sở giả định
  trong bài toán này — dùng thay thế vì không tìm được số của đúng
  cơ sở, cần Vinmec xác nhận lại.)

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
