# Risk Register — AI Gym Coach (v1.0)
[Last updated: 07/05/2026 by Founder]

*Giả định Burn Rate: $5,000/tháng.*
*Quy đổi: Mất $5,000 = 1 tháng runway. Mất $25,000 = 5 tháng runway.*

## 1. Customer-facing Risk (Rủi ro Tương tác Khách hàng)
- **Tình huống:** AI chấm sai tư thế nguy hiểm.
- **If** hệ thống MediaPipe nhận diện sai góc lưng (back rounding) trong bài Deadlift nhưng AI Coaching (Haiku) lại phản hồi "Form chuẩn",
- **Then** khách hàng tập tạ nặng theo, bị chấn thương thoát vị đĩa đệm, sau đó bóc phốt ứng dụng trên các group Gym lớn (Vietnam Fitness) + đòi bồi thường chi phí y tế.
- **Leading to:** Tốn $10,000 chi phí xử lý khủng hoảng, bồi thường y tế và sụt giảm 50% doanh thu (MRR) trong 3 tháng (~$15,000). Tổng thiệt hại: $25,000 = **5 tháng runway**.
- **Likelihood (1-5):** 3 (Có thể xảy ra do thuật toán ML TIP-058c Calibration chưa hoàn thiện).
- **Impact (1-5):** 4 (Mất 3-6 tháng runway).
- **Score:** 12 (Watch / Mitigate).

## 2. Vendor Risk (Rủi ro Bên thứ ba)
- **Tình huống:** LLM Vendor thay đổi chính sách.
- **If** Anthropic bất ngờ cấm ứng dụng "Medical/Physical Coaching" trên model Haiku của họ, hoặc tăng giá API lên 10x,
- **Then** tính năng LLM Coaching commentary bị sập toàn bộ hệ thống, hàng loạt user Pro (đã trả tiền qua SePay) yêu cầu refund vì phân tích bị lỗi chữ.
- **Leading to:** Phải refund cho user Pro (~$4,000) và tốn 2 tuần dev để migrate sang model khác (OpenAI/Gemini), mất chi phí cơ hội. Tổng thiệt hại: ~$5,000 = **1 tháng runway**.
- **Likelihood (1-5):** 2 (Anthropic ít khi cấm hoàn toàn mảng fitness nhưng vẫn có thể đổi ToS đột ngột).
- **Impact (1-5):** 1 (Mất <1 tháng runway).
- **Score:** 2 (Accept / Bỏ qua hàng ngày).

## 3. Founder-bandwidth Risk (Rủi ro Nguồn lực Founder)
- **Tình huống:** Sập queue phân tích khi Founder không có mặt.
- **If** Redis queue bị đầy bộ nhớ (OOM) hoặc Worker (Railway) treo đúng lúc Founder (người duy nhất nắm backend) đi du lịch không mang laptop hoặc bị ốm nằm viện 3 ngày,
- **Then** hàng ngàn video upload bị kẹt ở trạng thái "Processing", không có ai restart service, khách hàng đồng loạt đánh giá 1 sao.
- **Leading to:** Mất toàn bộ user mới trong tuần đó, 20% user Pro hủy sub, tốn $15,000 chi phí marketing để lấy lại uy tín. Tổng thiệt hại: $15,000 = **3 tháng runway**.
- **Likelihood (1-5):** 4 (Hệ thống xử lý video nặng, dễ quá tải queue, single point of failure).
- **Impact (1-5):** 3 (Mất 3 tháng runway).
- **Score:** 12 (Mitigate).
