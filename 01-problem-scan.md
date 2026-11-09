# 01-problem-scan

**Tên nhóm:** The Hive

**Thành viên:**

1. Đoàn Xuân Duy Bảo - doanxuanduybao96@gmail.com
2. Triệu Quốc Khánh - trieukhanh25032005@gmail.com
3. Vũ Quốc Đạt - dvuquoc60@gmail.com
4. Vũ Tiến Hùng - hungvu.education@gmail.com
5. Hoàng Đức An - anduchoang2701@gmail.com

---
# 🔍 Phase 1 — SCAN (Cá nhân, 20 min)

Hãy sử dụng **4 Lenses** dưới đây để quét qua hoạt động vận hành của các công ty thành viên Vingroup. Ghi lại **ít nhất 5 bài toán/bottleneck** thực tế.

### 4 Lenses tìm bài toán AI cho Vingroup:
1. **Lặp lại (Repetitive):** Tác vụ lặp đi lặp lại nhiều lần hằng ngày. (Ví dụ: So khớp hóa đơn sạc điện tại VinFast, route lại chuyến taxi tại Xanh SM).
2. **Tốn thời gian (Time-consuming):** Tác vụ ngốn thời gian xử lý thủ công của nhân viên. (Ví dụ: Soạn thảo phản hồi đánh giá 1-star của cư dân Vinhomes).
3. **AI có thể tốt hơn (AI-upgrade):** Dịch vụ khách hàng hiện tại còn chậm hoặc phản hồi rập khuôn. (Ví dụ: Chatbot CSKH Vinpearl hỗ trợ đặt vé vui chơi).
4. **Pain từ người khác (Stakeholder Pain):** Bottleneck khiến khách hàng hoặc nhân viên thực địa phàn nàn. (Ví dụ: Tài xế Xanh SM phàn nàn về việc hệ thống gợi ý điểm đón khách không chính xác).

### 📝 List bài toán của tôi:
| # | Subsidiary (VinFast/Xanh SM...) | Lens | Mô tả ngắn bài toán |
|---|----------------------------------|------|---------------------|
| 1 |**Xanh SM** |AI-upgrade |Hệ thống phân tích khu vực có nhu cầu đặt xe cao theo ngày/giờ/thời tiết/sự kiện |
| 2 |**VinFast** |Tốn thời gian |Nhân viên kiếm thử tại nhà máy thủ công kiểm tra chất lượng xe (mất khoảng 1h - 2h để kiểm tra từng chi tiết như ngoại hình, nội thất,...) |
| 3 |**VinFast** |Tốn thời gian |Khi khách hàng đưa xe tới trung tâm bảo hành để sửa chữa, nhân viên kỹ thuật thủ công kiếm tra và xác định nguyên nhân lỗi (mất khoảng 2-3h/1 xe) |
| 4 |**Vinhomes** |Lặp lại |Nhân viên chăm sóc khách hàng tiếp nhận các phản ảnh người dân và phân loại yêu cầu như kỹ thuật, vệ sinh, an ninh,..., từ đó chuyển đến bộ phận phụ trách |
| 5 |**Vinhomes** |AI-upgrade |Một chatbot giúp người dùng tìm kiếm căn hộ theo nhu cầu như vị trí, giá tiền, không gian phòng (cần phòng ngủ, phòng khách,...) |

---

# 🃏 Phase 2 — QUICK-ASSESS (Cá nhân, 30 min)

Chọn **top 3 bài toán** từ danh sách trên và hoàn thiện **3 Quick Problem Cards** dưới đây (10 phút/card).

```
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1                                     │
│                                                             │
│ Bài toán: Tài xế XanhSM muốn nắm được thông tin khu vực có nhu cầu cao để tăng thu nhập │
│ Công ty thành viên: [x] Xanh SM                             │
│                                                             │
│ Ai đang đau (Actor)? Tài xế                                 │
│                                                             │
│ Workflow thủ công hiện tại (3-5 bước):                      │
│   1. Truy cập vào các diễn đàn ──> 2. Tìm kiếm thông tin khu vực đông khách ──> 3. Di chuyển xe tới địa điểm                 │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2 (⏱ 30 phút/lượt)      │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2 (Tự động hóa phân tích địa điểm đông khách -> Đưa ra những địa điểm gần nhất cho tài xế) │
│                                                             │
│ Đo thành công bằng gì (Metric có số)? Giảm thời gian tìm kiếm từ 30 phút - > 3 phút │
│                                                             │
│ Quick Architecture: [x] Agent (Tự động thu thập dữ liệu từ quá khứ, dữ liệu nhu cầu tùy vị trí, thời tiết,... và đưa ra gợi ý địa điểm)                                │
└─────────────────────────────────────────────────────────────┘
```
```
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #4                                    │
│                                                             │
│ Bài toán (1 câu): Nhân viên chăm sóc khách hàng tiếp nhận các phản ảnh người dân chuyển đến bộ phận phụ trách │
│ Công ty thành viên: [x] Vinhomes
│                                                             │
│ Ai đang đau (Actor)? Nhân viên chăm sóc khách hàng │
│                                                             │
│ Workflow thủ công hiện tại (3-5 bước):                      │
│   1. Tiếp nhận yêu cầu ──> 2. Đọc và phân tích yêu cầu ──> 3. Phân loại yêu cầu ──> 4. Gửi tới bộ phận phụ trách                   │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2 (⏱ 5 phút/lượt)      │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2-3 │
│                                                             │
│ Đo thành công bằng gì (Metric có số)? Giảm thời gian xử lí từ 5 phút -> 5 giấy│
│                                                             │
│ Quick Architecture: [x] LLM (Đọc và phân loại yêu cầu) │
└─────────────────────────────────────────────────────────────┘
```

```
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #5                                     │
│                                                             │
│ Bài toán (1 câu): Người dùng tìm kiếm căn hộ theo nhu cầu như vị trí, giá tiền, không gian phòng,...  │
│ Công ty thành viên: [x] Vinhomes  │
│                                                             │
│ Ai đang đau (Actor)? Người dùng muốn tìm căn hộ │
│                                                             │
│ Workflow thủ công hiện tại (3-5 bước):                      │
│   1. Xác định nhu cầu cá nhân ──> 2. Thủ công lựa chọn lọc theo nhu cầu trên trang tìm kiếm căn hộ ──> 3. Mở xem thông tin căn hộ ──> 4. Liệt kê các căn hộ phù hợp -> 5. Chọn ra căn hộ mong muốn                   │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2-3 (⏱ 5 phút/lượt)      │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2-3 │
│                                                             │
│ Đo thành công bằng gì (Metric có số)? Giảm thời gian tìm kiếm từ 5 phút -> 1 phút│
│                                                             │
│ Quick Architecture: [x] Agent │
└─────────────────────────────────────────────────────────────┘
```

---
