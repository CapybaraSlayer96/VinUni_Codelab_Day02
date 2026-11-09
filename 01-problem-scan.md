# 01-problem-scan

**Tên nhóm:** The Hive

**Thành viên:**

1. Đoàn Xuân Duy Bảo - doanxuanduybao96@gmail.com
2. Triệu Quốc Khánh - trieukhanh25032005@gmail.com
3. Vũ Quốc Đạt - dvuquoc60@gmail.com
4. Vũ Tiến Hùng - hungvu.education@gmail.com
5. Hoàng Đức An - anduchoang2701@gmail.com

---

## Phase 1 – Scan

Dùng 4 Lenses (Lặp lại, Tốn thời gian, AI-upgrade, Pain từ người khác) quét vận hành các công ty thành viên Vingroup. Chọn 5 case tốt nhất từ 10 case đã quét (5 VinFast, 5 Vinhomes).

| # | Subsidiary | Lens | Mô tả ngắn bài toán |
|---|------------|------|---------------------|
| 1 | Vinhomes | AI-upgrade | Smart Urban Resource Optimization: dự báo và phát hiện bất thường điện/nước tại đại đô thị từ IoT/SCADA, đề xuất lịch vận hành cho trung tâm điều hành. Tiền điện tham chiếu ~805,8 tỷ/năm (322,3 triệu kWh). |
| 2 | Vinhomes | Lặp lại | Customer Transaction Intelligence: nhân viên kiểm tra hồ sơ, theo dõi công nợ, chuẩn bị hồ sơ giấy chứng nhận sở hữu thủ công cho hàng chục nghìn hồ sơ/năm. |
| 3 | VinFast | Lặp lại | Regulatory Intelligence: nhóm Regulatory/Legal tìm kiếm, đối chiếu quy định đa thị trường với từng dòng xe (~8 giờ/người/tuần), dễ sót thay đổi hoặc hết hạn chứng nhận. |
| 4 | VinFast | Pain từ người khác | Vehicle Delivery Intelligence: điều phối logistics xe thành phẩm và bàn giao (ETA, tuyến vận chuyển, bãi xe, PDI). Nhiều bên phối hợp, xe chờ bãi, giao trễ, chi phí lưu kho/vận tải chưa tối ưu. |
| 5 | Vinhomes | AI-upgrade | Procurement Intelligence: phát hiện chênh lệch giá vật tư giữa dự án, điều khoản hợp đồng bất thường, thanh toán không khớp nghiệm thu. |

## Phase 2 — QUICK-ASSESS: 3 Quick Problem Cards

### Quick Problem Card #1

```
QUICK PROBLEM CARD #1

Bài toán (1 câu): Chuyển trung tâm vận hành đô thị từ giám sát theo ngưỡng sang dự báo phụ tải điện/nước, phát hiện bất thường và đề xuất lịch vận hành tối ưu.
Công ty thành viên: [ ] VinFast  [ ] Xanh SM  [x] Vinhomes  [ ] Vinmec  [ ] Khác

Ai đang đau (Actor)? Nhân viên trung tâm vận hành đô thị và kỹ thuật (Operations, Engineering) tại các khu đô thị.

Workflow thủ công hiện tại (3-5 bước):
  1. Thu thập dữ liệu meter/SCADA/BMS ──> 2. Theo dõi tiêu thụ theo tòa nhà, khu vực, thiết bị ──> 3. So với định mức/ngưỡng và lịch sử ──> 4. Tìm nguyên nhân (tải, thời tiết, lịch vận hành, rò rỉ) ──> 5. Điều chỉnh lịch vận hành

Bước nào tốn thời gian/lỗi nhất? Bước 3–4: phát hiện bất thường và tìm nguyên nhân chỉ dựa trên ngưỡng cảnh báo (⏱ chưa có số, cần time study tại trung tâm vận hành).
AI có thể nhảy vào hỗ trợ ở bước nào? Bước 3–5: dự báo phụ tải, phát hiện bất thường, đề xuất lịch vận hành; người duyệt, không tự điều khiển thiết bị.

Đo thành công bằng gì (Metric có số)?
  Giảm 1–3% điện năng so với baseline chuẩn hóa theo thời tiết/occupancy
  ──> 8,1–24,2 tỷ VND/năm (322,3 triệu kWh × 2.500 VND/kWh, giá điện là giả định).
  KPI phụ: thời gian phát hiện bất thường, false-alarm rate, tỷ lệ khuyến nghị được chấp thuận.

Quick Architecture: [ ] No AI  [x] Rule  [ ] LLM  [ ] Agent
```

### Quick Problem Card #2

```
QUICK PROBLEM CARD #2

Bài toán (1 câu): Tự động kiểm tra hồ sơ, đối chiếu hợp đồng/thanh toán và chuẩn bị hồ sơ giấy chứng nhận sau bán hàng để giảm kiểm tra lặp lại.
Công ty thành viên: [ ] VinFast  [ ] Xanh SM  [x] Vinhomes  [ ] Vinmec  [ ] Khác

Ai đang đau (Actor)? Nhân viên Customer Procedures Division (thủ tục, công nợ, giấy chứng nhận sở hữu).

Workflow thủ công hiện tại (3-5 bước):
  1. Tiếp nhận hợp đồng sau giao dịch ──> 2. Đồng bộ khách hàng, căn hộ, lịch thanh toán ──> 3. Theo dõi công nợ, xác nhận thanh toán ──> 4. Kiểm tra hồ sơ đủ/hợp lệ ──> 5. Chuẩn bị hồ sơ giấy chứng nhận, chuyển Operations

Bước nào tốn thời gian/lỗi nhất? Bước 4: đối chiếu chứng từ với hợp đồng/thanh toán, nhiều vòng bổ sung (⏱ giả định tiết kiệm 0,5–1 giờ/hồ sơ, chưa đo thực tế).
AI có thể nhảy vào hỗ trợ ở bước nào? Bước 4–5: OCR trích xuất trường, matching hợp đồng–thanh toán, RAG tra checklist, lập danh sách thiếu/sai cho nhân viên duyệt.

Đo thành công bằng gì (Metric có số)?
  Giảm thời gian xử lý hồ sơ 0,5–1 giờ/hồ sơ trên 20.000 hồ sơ/năm (giả định)
  ──> 1,2–3,6 tỷ VND/năm. KPI phụ: tỷ lệ hồ sơ đầy đủ lần đầu, số vòng bổ sung, đúng SLA.

Quick Architecture: [ ] No AI  [ ] Rule  [ ] LLM  [x] Agent
```

### Quick Problem Card #3

```
QUICK PROBLEM CARD #3

Bài toán (1 câu): Phát hiện chênh lệch giá vật tư, điều khoản hợp đồng bất thường và thanh toán không khớp nghiệm thu trên nhiều dự án.
Công ty thành viên: [ ] VinFast  [ ] Xanh SM  [x] Vinhomes  [ ] Vinmec  [ ] Khác

Ai đang đau (Actor)? Nhân viên Supply/Đấu thầu, Phòng Thanh Quyết toán và Finance.

Workflow thủ công hiện tại (3-5 bước):
  1. Gửi yêu cầu mua sắm, báo giá, mời thầu ──> 2. Chọn nhà thầu, ký hợp đồng/phụ lục ──> 3. Nhận hàng, nghiệm thu ──> 4. Kiểm tra hợp đồng, khối lượng, đơn giá, hóa đơn ──> 5. Thanh toán

Bước nào tốn thời gian/lỗi nhất? Bước 4: so giá giữa dự án phải chuẩn hóa mô tả vật tư, thông số, khối lượng, thời điểm; đối chiếu chéo hợp đồng–PO–nghiệm thu–hóa đơn (⏱ chưa có số, cần time study).
AI có thể nhảy vào hỗ trợ ở bước nào? Bước 4: chuẩn hóa vật tư, phát hiện giá bất thường, matching hợp đồng–nghiệm thu–thanh toán, cảnh báo cho người duyệt.

Đo thành công bằng gì (Metric có số)?
  Giảm 0,3–0,8% chi phí bất thường được xác minh trên 1.000 tỷ VND mua sắm/năm (giả định)
  ──> 3–8 tỷ VND/năm. KPI phụ: số cảnh báo được xác minh, lỗi mismatch, thời gian rà soát hồ sơ.

Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM  [ ] Agent
```
