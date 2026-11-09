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

Luồng hiện tại (chatbot kịch bản cố định):

[1] Khách nhắn chatbot (app/kênh chat) hoặc gọi tổng đài 1555 ....................... ~1 phút (GĐ)
      ↓
[2] Chatbot khớp từ khóa/menu theo kịch bản cố định ................................. ~1 phút (GĐ)
      ↓   🔴 BOTTLENECK 1: câu hỏi ngoài kịch bản → bot không hiểu, trả lời lặp/sai
[3] Bot không xử lý được → chuyển nhân viên CSKH ..................................... ~2 phút (GĐ)
      ↓   🔄 HANDOFF 1: Chatbot → Nhân viên CSKH (khách thường phải kể lại từ đầu)
[4] Khách xếp hàng chờ nhân viên ..................................................... ~8 phút (GĐ)
      ↓   🔴 BOTTLENECK 2: chờ lâu, tải dồn vào nhân viên cho cả câu hỏi đơn giản
[5] Nhân viên tra cứu chính sách/FAQ, hỏi lại thông tin, trả lời ..................... ~6 phút (GĐ)
      ↓   🔄 HANDOFF 2: CSKH → bộ phận chuyên trách (hoàn tiền, an toàn, đối tác lái xe)
[6] Tạo/cập nhật ticket, đóng case ................................................... ~3 phút (GĐ)

**Tổng cộng = ~21 phút/lượt phải chuyển người (GĐ)**
(câu hỏi đơn giản có trong kịch bản chỉ ~2 phút; thời gian dài nằm ở nhóm ngoài kịch bản)

## 3.2. Problem Statement (6-field) & Metrics

| Field | Nội dung chi tiết |
|---|---|
| **1. Actor / Operator** | Nhân viên CSKH (chat + tổng đài 1555, 24/7) của Xanh SM; khách hàng đi xe; bộ phận chuyên trách (hoàn tiền, an toàn) nhận case chuyển tiếp. |
| **2. Current Workflow** | Chatbot trả lời theo kịch bản/menu cố định. Câu hỏi ngoài kịch bản được chuyển nhân viên, nhân viên tra FAQ/chính sách, hỏi lại khách, xử lý và tạo ticket thủ công. |
| **3. Bottleneck** | (a) Bot không hiểu câu hỏi tự do, diễn đạt khác nhau; (b) khách chờ nhân viên lâu; (c) nhân viên mất thời gian cho câu hỏi lặp lại, có đáp án sẵn trong chính sách/FAQ. Đây là bài toán hiểu và sinh ngôn ngữ tự nhiên, kịch bản cố định không đáp ứng tốt. |
| **4. Business Impact** | Quy mô: Green SM chiếm 54,51% thị phần Q1/2026; toàn ngành 153,26 triệu chuyến/quý (Mordor Intelligence qua LSVN) → ~83,5 triệu chuyến/quý ≈ **~0,9 triệu chuyến/ngày** (tự tính). Chi phí liên hệ: tổng đài 1555 hoạt động 24/7, 1.000đ/phút (nguồn Green SM). Tổn thất: thời gian chờ của khách, chi phí nhân sự CSKH, rủi ro CSAT thấp. **Chưa có baseline** số lượt liên hệ/ngày, tỷ lệ tự giải quyết hiện tại, thời gian chờ trung bình; đây là việc phải đo đầu tiên. Lưu ý: 0,9 triệu chuyến/ngày là quy mô chuyến, không phải số lượt liên hệ CSKH. |
| **5. Success Metric** | *(GĐ, chốt sau khi có baseline thực)* • ≥50% câu hỏi ngoài kịch bản (nhóm không nhạy cảm) được bot tự giải quyết, không chuyển người • Thời gian phản hồi đầu tiên ≤10 giây • Giảm thời gian chờ trung bình của nhóm này từ ~8 phút xuống ≤2 phút • CSAT của hội thoại do bot xử lý ≥ mức hiện tại của nhân viên (không thấp hơn quá 0,2 điểm) • Tỷ lệ trả lời sai chính sách (kiểm bằng mẫu) ≤2%; case nhạy cảm chuyển người đúng 100% trên tập test. |
| **6. Operational Boundary** | **Được phép:** trả lời dựa trên chính sách/FAQ chính thức đã phê duyệt (RAG, kèm trích nguồn nội bộ); phân loại ý định; thu thập thông tin trước khi chuyển người; tóm tắt hội thoại cho nhân viên. **TUYỆT ĐỐI không:** bịa chính sách/giá/điều khoản; tự quyết định hoàn tiền, bồi thường; xử lý tình huống an toàn (tai nạn, quấy rối, khẩn cấp) mà không chuyển người ngay; hứa hẹn ngoài chính sách; yêu cầu thông tin nhạy cảm (mật khẩu, OTP). **Cần duyệt/chuyển người:** hoàn tiền, an toàn, khiếu nại tài chính, khách bực tức, bot không chắc (dưới ngưỡng tin cậy), câu hỏi ngoài phạm vi nội dung chính sách. |

## 3.3. Future-State Flow & AI Fit

* **AI-Fit Matrix:** [ ] Rule / State-Machine  [x] **LLM Feature**  [ ] Agentic Loop
  *(LLM + truy xuất chính sách/FAQ (RAG) để trả lời câu hỏi tự do. Không dùng Agentic Loop vì bot không tự thực hiện hành động như hoàn tiền; luật chuyển người cho case nhạy cảm vẫn là rule cứng đứng trước LLM.)*

* **Future-State Flow:**

Khách nhắn chat / gọi 1555
      ↓
Rule cứng: phát hiện từ khóa nhạy cảm/an toàn → chuyển người NGAY (bỏ qua LLM)
      ↓
🔵 AI Step 1: Phân loại ý định + mức nhạy cảm
🔵 AI Step 2: Truy xuất chính sách/FAQ chính thức, soạn câu trả lời kèm độ tin cậy
      ↓
   Độ tin cậy cao + case thường → bot trả lời khách → hỏi "đã giải quyết chưa?"
   Độ tin cậy thấp / nhạy cảm → chuyển người kèm tóm tắt hội thoại
      ↓
🟢 Human Step (HITL): Nhân viên CSKH nhận case nhạy cảm/không chắc (đã có sẵn tóm tắt,
   không cần khách kể lại), xử lý hoàn tiền/an toàn; duyệt mẫu hội thoại bot hằng tuần
      ↓
Ghi ticket, lưu kết quả (đã giải quyết / chuyển người / khách đánh giá) → cải thiện FAQ, prompt

  ↩️ Fallback:
  • LLM lỗi/timeout hoặc độ tin cậy thấp → trả lời mẫu "em chuyển anh/chị sang nhân viên"
    và chuyển người, không đoán
  • Không tìm thấy nội dung chính sách phù hợp → không trả lời tự do, chuyển người
  • Khách yêu cầu gặp người / lặp lại 2 lần chưa hài lòng → chuyển người ngay
  • LLM ngừng hoạt động → chatbot kịch bản hiện tại chạy như cũ

# 🏁 Phase 5 — EVALUATE

### AI Readiness Checklist:
1. [~] Chúng tôi có sẵn dữ liệu mẫu/logs sạch để test?
   → Có log chat và FAQ (Data 4/5), nhưng chưa xác nhận chất lượng, độ cập nhật của chính sách
     và mức độ che thông tin cá nhân; cần kiểm tra trước khi dùng.
2. [x] Rủi ro khi AI sai có nằm trong tầm kiểm soát (qua HITL hoặc Fallback)?
   → Kiểm soát được nếu giữ ranh giới chặt: rule cứng chuyển người cho case nhạy cảm, chỉ trả lời từ
     chính sách đã duyệt, có fallback và chạy shadow mode trước. Rủi ro cao hơn case điện/nước vì
     bot nói chuyện trực tiếp với khách.
3. [~] Stakeholders sẵn sàng thay đổi quy trình làm việc cũ?
   → Chưa xác nhận; cần CSKH đồng ý quy trình nhận case kèm tóm tắt và bộ phận chính sách
     chịu trách nhiệm cập nhật nội dung nguồn.

### Quyết định cuối cùng của Ban Giám Đốc Vin Smart Future:
[x] **GO (Bắt đầu xây dựng Prototype):** scope hẹp, có điều kiện
[ ] **NOT YET**
[ ] **NO-GO**

**Justification:**
> GO với scope hẹp: prototype chỉ trả lời nhóm FAQ/chính sách không nhạy cảm (giá cước, quy trình
> đặt/hủy chuyến, đồ thất lạc, hướng dẫn app), chạy shadow mode 2–4 tuần (bot soạn nháp, người
> quyết định) rồi mới cho trả lời trực tiếp một phần lưu lượng. Hoàn tiền và an toàn luôn chuyển người.
> • Bằng chứng nhu cầu: WOS 88/100; quy mô lớn (~0,9 triệu chuyến/ngày, tự tính); AI Advantage 5/5
>   vì câu hỏi tự do là điểm mạnh của LLM mà kịch bản cố định không xử lý được.
> • Đo lường rõ: tỷ lệ tự giải quyết, thời gian phản hồi, CSAT đều đo được từ log.
> • Rủi ro kiểm soát được: RAG trên chính sách chính thức, rule cứng cho case nhạy cảm, fallback về
>   người và về chatbot cũ.
> • Điều kiện để tiếp tục (nếu không đạt thì chuyển sang NOT YET): (1) đo baseline trong 2 tuần đầu
>   (lượt liên hệ/ngày, tỷ lệ tự giải quyết, thời gian chờ); (2) bộ FAQ/chính sách được duyệt và có
>   người chịu trách nhiệm cập nhật; (3) tích hợp được ticket (feasibility 3/5); (4) qua 3 Gate
>   (Evidence of Pain, AI Necessity, Data Access) vì điểm WOS mới là đánh giá sơ bộ từ thông tin công khai.
> • Rủi ro cần lưu ý: chủ yếu giảm chi phí, không trực tiếp tăng doanh thu (Impact 4/5); nguy cơ
>   bịa chính sách là rủi ro thương hiệu, nên ngưỡng "trả lời sai ≤2%" là điều kiện bắt buộc trước
>   khi mở rộng.