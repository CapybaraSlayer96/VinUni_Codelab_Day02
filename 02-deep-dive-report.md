# 02-deep-dive-report

**Tên nhóm:** The Hive

**Thành viên:**

1. Đoàn Xuân Duy Bảo - doanxuanduybao96@gmail.com
2. Triệu Quốc Khánh - trieukhanh25032005@gmail.com
3. Vũ Quốc Đạt - dvuquoc60@gmail.com
4. Vũ Tiến Hùng - hungvu.education@gmail.com
5. Hoàng Đức An - anduchoang2701@gmail.com

---

# 🏗️ Phase 3 — DEEP-DIVE (Nhóm, 85 min)

## 3.1. Current-State Workflow Mapping

Luồng hiện tại (giám sát theo ngưỡng cảnh báo):

[1] Công tơ/cảm biến điện-nước → SCADA / V-PMS thu thập dữ liệu (tự động) ........ ~5 phút (GĐ)
      ↓
[2] Dashboard hiển thị chỉ số, so với ngưỡng cố định ........................... ~10 phút (GĐ)
      ↓   🔴 BOTTLENECK 1: ngưỡng tĩnh chỉ báo SAU KHI vượt; không thấy trước
      ↓      nhu cầu, rò rỉ nhỏ kéo dài hay thiết bị chạy sai nhưng chưa vượt ngưỡng
[3] Nhân viên trung tâm nhận cảnh báo, đối chiếu dữ liệu, gọi BQL khu .......... ~30 phút (GĐ)
      ↓   🔄 HANDOFF 1: Trung tâm vận hành → Ban quản lý/đội kỹ thuật tại khu đô thị
      ↓   🔴 BOTTLENECK 2: xác minh thủ công qua 32 khu đô thị, dễ báo nhầm/bỏ sót
[4] Điều phối kỹ thuật ra hiện trường .......................................... ~45 phút (GĐ)
      ↓   🔄 HANDOFF 2: Hiện trường → Trung tâm (phản hồi kết quả)
[5] Kiểm tra, xử lý (rò rỉ, chỉnh thiết bị, chỉnh lịch chạy) ................... ~60 phút (GĐ)
      ↓
[6] Ghi nhận, báo cáo thủ công ................................................. ~15 phút (GĐ)

Lịch vận hành thiết bị (bơm, HVAC, chiếu sáng công cộng): đặt theo lịch cố định/kinh
nghiệm, điều chỉnh thủ công → 🔴 BOTTLENECK 3: không tối ưu theo phụ tải thực tế.

**Tổng cộng = ~165 phút/lượt cảnh báo (GĐ)**, chưa tính độ trễ phát hiện
(có thể hàng giờ đến hàng ngày với rò rỉ nhỏ hoặc thiết bị chạy sai).

## 3.2. Problem Statement (6-field) & Metrics

| Field | Nội dung chi tiết |
|---|---|
| **1. Actor / Operator** | Nhân viên Trung tâm vận hành (giám sát điện/nước) và kỹ thuật viên/BQL tại 32 khu đô thị dùng V-PMS. |
| **2. Current Workflow** | Dữ liệu từ IoT/SCADA hiện có hiển thị trên dashboard; hệ thống chỉ cảnh báo theo ngưỡng. Nhân viên xác minh thủ công, gọi điều phối kỹ thuật, xử lý và ghi nhận. Lịch vận hành thiết bị đặt theo kinh nghiệm/lịch cố định. |
| **3. Bottleneck** | (a) Ngưỡng cố định không thấy trước nhu cầu, rò rỉ hay thiết bị chạy sai; (b) xác minh cảnh báo thủ công trên 32 khu; (c) không có công cụ dự báo/tối ưu lịch vận hành. Đây là bài toán dự báo và phát hiện bất thường, rule theo ngưỡng không đáp ứng tốt. |
| **4. Business Impact** | Baseline công bố (Vinhomes AR 2025, tr.150–151): **322,3 triệu kWh/năm**, 1.253,1 TJ năng lượng, 32 khu đô thị. Nếu tiết kiệm 1–3% (giả định) và giá điện 2.500 VND/kWh (giả định): **8,1–24,2 tỷ VND/năm** (322,3 tr kWh × 2.500 = 805,8 tỷ; 1% = 8,1 tỷ; 3% = 24,2 tỷ). Lưu ý: 805,8 tỷ là con số quy đổi, không phải chi phí điện thực; cần tách điện khu vực công cộng khỏi điện riêng của cư dân để tránh tính trùng. |
| **5. Success Metric** | *(GĐ, chốt lại sau khi có baseline thực)* • Giảm ≥1% kWh khu vực công cộng tại khu pilot sau 3 tháng, chuẩn hóa theo thời tiết/occupancy • MAPE dự báo phụ tải ngày kế tiếp ≤10% • Phát hiện ≥80% sự cố bất thường đã biết trong dữ liệu lịch sử, tỷ lệ báo nhầm ≤20% • Giảm thời gian từ phát hiện đến xác minh từ ~40 phút xuống ≤10 phút • ≥60% khuyến nghị được nhân viên duyệt/áp dụng. |
| **6. Operational Boundary** | **Được phép:** đọc dữ liệu (read-only), dự báo phụ tải, gắn cờ bất thường kèm lý do, đề xuất lịch vận hành tối ưu. **TUYỆT ĐỐI không:** tự điều khiển/ghi lệnh xuống thiết bị SCADA/OT, tự thay đổi lịch vận hành, tự đóng/mở bơm hay tải. **Cần duyệt:** mọi khuyến nghị đều do nhân viên vận hành phê duyệt từng cái trước khi thực hiện (thủ công qua hệ thống hiện có). |

## 3.3. Future-State Flow & AI Fit

* **AI-Fit Matrix:** [ ] Rule / State-Machine  [x] **LLM Feature**  [ ] Agentic Loop
  *(Lõi là mô hình ML: dự báo chuỗi thời gian + phát hiện bất thường. Nếu thêm lớp LLM thì chỉ để diễn giải cảnh báo/khuyến nghị bằng ngôn ngữ tự nhiên. Không dùng Agentic Loop vì AI không tự hành động. Rule theo ngưỡng vẫn giữ làm lớp an toàn.)*

* **Future-State Flow:**

[SCADA/V-PMS hiện có] → dữ liệu điện/nước, thời tiết, occupancy (read-only)
      ↓
🔵 AI Step 1: Dự báo phụ tải điện/nước (ngày/tuần)
🔵 AI Step 2: Phát hiện bất thường (rò rỉ, thiết bị chạy sai, tiêu thụ lệch dự báo)
🔵 AI Step 3: Đề xuất lịch vận hành tối ưu + giải thích lý do/độ tin cậy
      ↓
🟢 Human Step (HITL): Nhân viên Trung tâm vận hành xem, duyệt / sửa / từ chối TỪNG khuyến nghị
      ↓
Thực hiện qua quy trình & hệ thống hiện có (không qua AI) → ghi nhận kết quả
      ↓
Vòng phản hồi: quyết định duyệt/từ chối được lưu để cải thiện mô hình

  ↩️ Fallback:
  • Dữ liệu thiếu/lỗi cảm biến hoặc độ tin cậy thấp → AI không phát khuyến nghị, gắn nhãn
    "không đủ tin cậy", chuyển về cảnh báo theo ngưỡng như hiện tại
  • AI/mô hình ngừng hoạt động → hệ thống giám sát hiện có chạy bình thường (AI chỉ là lớp bổ sung)
  • Khuyến nghị bất thường ngoài biên an toàn → chặn và leo thang cho kỹ sư trưởng

# 🏁 Phase 5 — EVALUATE

### AI Readiness Checklist:
1. [~] Chúng tôi có sẵn dữ liệu mẫu/logs sạch để test? 
   → Có nền tảng (V-PMS/SCADA ở 32 khu, có số liệu kWh), nhưng CHƯA xác nhận chất lượng,
     độ dài lịch sử, độ phủ meter; cần xin truy cập OT (Feasibility 3/5).
2. [x] Rủi ro khi AI sai có nằm trong tầm kiểm soát (qua HITL hoặc Fallback)?
   → Có: AI chỉ đề xuất, người duyệt từng khuyến nghị, không điều khiển thiết bị, có fallback về ngưỡng.
3. [~] Stakeholders sẵn sàng thay đổi quy trình làm việc cũ?
   → Chưa xác nhận; thay đổi nhỏ (thêm 1 bước duyệt khuyến nghị) nên khả thi, cần cam kết từ Trung tâm vận hành.

### Quyết định cuối cùng của Ban Giám Đốc Vin Smart Future:
[x] **GO (Bắt đầu xây dựng Prototype):** scope hẹp, có điều kiện
[ ] **NOT YET**
[ ] **NO-GO**

**Justification:**
> GO với scope hẹp: pilot 1–2 khu đô thị, chỉ dự báo phụ tải + phát hiện bất thường ở chế độ
> khuyến nghị (read-only), chưa làm tối ưu lịch ở giai đoạn đầu.
> • Bằng chứng nhu cầu: WOS 89/100 (High Opportunity); baseline công bố rõ (322,3 triệu kWh,
>   1.253,1 TJ, 32 khu); ROI kịch bản lớn nhất trong 3 case (8,1–24,2 tỷ VND/năm, giả định).
> • AI cần thiết thật: dự báo và phát hiện bất thường là bài toán rule theo ngưỡng không giải tốt.
> • Rủi ro thấp: AI không điều khiển thiết bị, có HITL và fallback; chi phí pilot nhỏ vì chạy trên
>   hạ tầng IoT/SCADA sẵn có.
> • Đo lường rõ: so sánh trước–sau bằng kWh/chi phí, chuẩn hóa theo thời tiết/occupancy.
> • Điều kiện để tiếp tục (nếu không đạt thì chuyển sang NOT YET): (1) được cấp quyền đọc
>   dữ liệu SCADA/meter; (2) có ≥12 tháng lịch sử đủ sạch ở khu pilot; (3) Vinhomes xác nhận giá điện
>   và tách điện công cộng khỏi điện cư dân; (4) vượt 3 Gate (Evidence of Pain, AI Necessity, Data
>   Access) vì điểm WOS mới là đánh giá sơ bộ từ thông tin công khai.
> • Rủi ro cần lưu ý: mức tiết kiệm 1–3% chưa được xác nhận; khoảng cách 89 vs 82/81 là chấm định tính.
