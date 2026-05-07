# Risk Register v2 (AI-Augmented) — AI Gym Coach
[Generated via AI CRO Audit Prompt]

*Giả định: Startup 5 người, 12 tháng runway. Burn Rate: $5,000/tháng. Tổng vốn: $60,000.*

## TOP 10 RISKS

**RISK 1: False Positive Injury**
- Type: Customer-facing
- If: MediaPipe (TIP-058c) bỏ sót lỗi "back rounding" do góc quay tối / Then: Khách hàng thực hiện Deadlift tạ nặng theo, bị chấn thương cột sống và kiện startup.
- Leading to: Bồi thường y tế và sụt giảm 50% doanh thu (MRR) = 5 months of runway.
- Likelihood (1-5) / Impact (1-5 by runway): L: 4 / I: 5
- Mitigation: 
  1. Thêm Disclaimer cực lớn "Chỉ tham khảo, không thay thế y tế" bắt buộc check trước upload.
  2. Implement thuật toán tính "Confidence Score" cho video. Dưới 70% -> Từ chối chấm điểm.
  3. Giới hạn bài tập rủi ro cao (Deadlift/Squat) chỉ dành cho Pro user (đã đồng ý ToS chặt chẽ).

**RISK 2: Webhook Idempotency Exploit**
- Type: Vendor
- If: Hacker phát hiện endpoint webhook SePay thiếu cơ chế idempotency / Then: Chúng bắn fake request liên tục để tạo hàng ngàn tài khoản Pro miễn phí.
- Leading to: Mất doanh thu tương lai và tốn chi phí API/Supabase cho user rác = 3 months of runway.
- Likelihood (1-5) / Impact (1-5 by runway): L: 3 / I: 3
- Mitigation:
  1. Bắt buộc validate SePay signature trên mọi webhook request.
  2. Set constraint `UNIQUE` trên cột `order_id` trong database Supabase.
  3. Rate limit webhook endpoint bằng Upstash Redis (chỉ nhận tối đa 5 req/s từ IP của SePay).

**RISK 3: Single Point of Failure (Backend)**
- Type: Founder-bandwidth
- If: Redis queue bị OOM hoặc MediaPipe Worker trên Railway sập đúng lúc Founder duy nhất bị ốm 3 ngày / Then: Toàn bộ quá trình phân tích video bị kẹt, app ngưng hoạt động hoàn toàn.
- Leading to: Bão review 1 sao, 30% user hủy gói Pro = 2 months of runway.
- Likelihood (1-5) / Impact (1-5 by runway): L: 4 / I: 2
- Mitigation:
  1. Setup free UptimeRobot gọi điện/nhắn SMS thẳng cho Founder khi queue depth vượt ngưỡng.
  2. Viết SOP (1 trang Notion) hướng dẫn nút "Restart Railway" và share cho một Co-founder khác.
  3. Code logic auto-restart Railway Worker nếu queue timeout quá 10 phút.

**RISK 4: Video Privacy Leak (PDPA)**
- Type: Regulatory
- If: Hacker lấy được link bucket `videos-raw` từ Supabase hoặc người nội bộ tuồn video khách hàng tập gym lên mạng / Then: Vi phạm nghiêm trọng quyền riêng tư (Luật PDPA VN).
- Leading to: Phạt hành chính, kiện tụng và khủng hoảng truyền thông khai tử thương hiệu = 12 months of runway (Death).
- Likelihood (1-5) / Impact (1-5 by runway): L: 2 / I: 5
- Mitigation:
  1. Viết RLS policy chặn tuyệt đối tải xuống video raw từ UI Admin.
  2. Chỉnh Cronjob tự xóa raw video sau 24h thay vì 30 ngày (chỉ giữ bản skeleton overlay).
  3. Không log PII (Personal Identifiable Information) trong bảng `audit_log`.

**RISK 5: Anthropic Policy Ban**
- Type: Vendor
- If: Anthropic cập nhật ToS cấm dùng Claude 3 Haiku cho "Tư vấn y tế/vật lý trị liệu" / Then: Tính năng Coaching bị khóa đột ngột.
- Leading to: Hoàn tiền hàng loạt cho user Pro và tốn công dev viết lại hệ thống = 1 month of runway.
- Likelihood (1-5) / Impact (1-5 by runway): L: 3 / I: 1
- Mitigation:
  1. Xây dựng LLM Adapter Layer trong FastAPI (không hardcode Anthropic).
  2. Chuẩn bị sẵn API key OpenAI/Gemini đã nạp tiền làm fallback.
  3. Code sẵn rule-based coaching từ `exercise_cues` DB phòng khi AI sập.

**RISK 6: MediaPipe OOM Timeout**
- Type: Vendor
- If: Hạ tầng cơ bản của Railway không chịu nổi nhiều instances của MediaPipe Heavy chạy đồng thời / Then: Worker bị OOM RAM liên tục, trả về lỗi timeout 120s cho khách.
- Leading to: Trải nghiệm cực tồi tệ, đánh giá kém, không ai mua gói Pro = 2 months of runway.
- Likelihood (1-5) / Impact (1-5 by runway): L: 5 / I: 2
- Mitigation:
  1. Giới hạn max concurrent workers = 1 để tránh OOM.
  2. Implement hàng đợi phía frontend: "Bạn đang ở vị trí thứ 3, vui lòng đợi 45s...".
  3. Nâng cấp Worker lên tier GPU ngay khi MRR đạt mốc $500/tháng.

**RISK 7: LLM Hallucinated Coaching**
- Type: Customer-facing
- If: Anthropic Haiku diễn dịch sai dữ liệu JSON và khuyên user "cần ưỡn lưng cong hơn nữa" khi Deadlift / Then: Khách hàng làm theo và gặp chấn thương nặng.
- Leading to: Khách hàng kiện và bóc phốt = 4 months of runway.
- Likelihood (1-5) / Impact (1-5 by runway): L: 3 / I: 4
- Mitigation:
  1. Đưa strict constraint vào System Prompt: "TUYỆT ĐỐI KHÔNG khuyên tăng độ cong của lưng trong bài Deadlift".
  2. Log 100% LLM response vào Helicone để Founder audit hàng tuần (Friday Ritual).
  3. Dùng RAG để LLM chỉ được phép bám sát nội dung chuyên môn từ Admin CMS.

**RISK 8: API Quota Exhaustion Attack**
- Type: Vendor
- If: Đối thủ dùng bot bypass frontend và spam API upload 10,000 video rác / Then: Supabase Storage và Anthropic API budget cạn kiệt trong vài giờ.
- Leading to: Downtime cho người dùng thật và bill thẻ tín dụng $2,000 = 1 month of runway.
- Likelihood (1-5) / Impact (1-5 by runway): L: 3 / I: 1
- Mitigation:
  1. Dời rate limit 10 uploads/giờ từ Redis lên Cloudflare WAF.
  2. Set hard spend limit trong Anthropic Console và Supabase Billing.
  3. Yêu cầu verify OTP qua email trước khi cho phép upload.

**RISK 9: Payment Support Overload**
- Type: Founder-bandwidth
- If: Hệ thống chuyển khoản ngân hàng (SePay) delay 5-10 phút / Then: Khách đã chuyển tiền nhưng chưa lên Pro lập tức spam email chửi rủa đòi refund.
- Leading to: Founder tốn 4h mỗi ngày làm customer support thay vì code = 1 month of runway.
- Likelihood (1-5) / Impact (1-5 by runway): L: 5 / I: 1
- Mitigation:
  1. Thêm cảnh báo rõ lúc thanh toán: "Ngân hàng có thể delay 5-10 phút, vui lòng không tắt màn hình".
  2. Thêm nút "Kiểm tra thanh toán" trên UI để ping lại SePay thủ công.
  3. Cài auto-reply email: "Nếu bạn đã chuyển khoản nhưng chưa lên Pro, vui lòng rep lại mã giao dịch".

**RISK 10: EU GDPR Violation (Technicality)**
- Type: Regulatory
- If: Một công dân EU đi du lịch Việt Nam, dùng app, sau đó yêu cầu xóa tài khoản nhưng `audit_log` của bạn vẫn giữ PII (email/IP) của họ / Then: Công ty vi phạm luật GDPR.
- Leading to: Rắc rối pháp lý nếu công ty muốn gọi vốn vòng Series A sau này = 1 month of runway.
- Likelihood (1-5) / Impact (1-5 by runway): L: 1 / I: 1
- Mitigation:
  1. Thêm Privacy Policy 1 trang nêu rõ dữ liệu được xử lý tại Việt Nam.
  2. Không thu thập hay lưu IP/Location của user vào database.
  3. Logic Delete Account phải anonymize (làm ẩn danh) email trong bảng `audit_log` thay vì soft-delete.
