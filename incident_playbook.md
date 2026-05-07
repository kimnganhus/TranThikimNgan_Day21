# Incident Playbook — AI Gym Coach
[Last updated: 07/05/2026 by Founder]

## Tình huống (Scenario)
9h30 sáng thứ Hai. Một user post video lên Facebook Group "Vietnam Fitness" (100K members): Video cho thấy ứng dụng AI Gym Coach báo "Form chuẩn 95 điểm" cho một cú Deadlift còng lưng rất nặng, kèm caption: "App rác suýt làm gãy lưng tao". Đã có 300 comments chửi rủa trong 45 phút. Tín hiệu viral cực xấu.

## 0–5 phút: VERIFY (Xác minh)
**Mục tiêu:** Xác nhận đây có phải là lỗi thật của AI Gym Coach hay là giả mạo (photoshop).
- **Hành động:** 
  1. Vào `Supabase Dashboard` -> bảng `analysis_sessions` và `video_assets`.
  2. Search user name hoặc tìm theo timestamp tải lên gần nhất.
  3. Xem điểm số `overall_score` và `analysis_issues` của session đó.
  4. Nếu điểm trong DB là cao nhưng trong video back rounding rõ ràng -> **Xác nhận sự cố do thuật toán MediaPipe bắt sai điểm.**

## 5–15 phút: STOP THE BLEEDING (Cầm máu)
**Quyết định:** **SOFT KILL (Fallback)**.
- **Lý do:** Tắt riêng chức năng chấm bài Deadlift thay vì tắt toàn bộ web (tránh sập doanh thu các bài Squat, Bench Press vẫn đang chạy tốt).
- **Hành động kỹ thuật:** Vào Vercel Environment Variables, đổi `ENABLE_DEADLIFT_AI=false`. Frontend sẽ tự động hiện thông báo "Tính năng chấm Deadlift đang bảo trì để nâng cấp độ chính xác" khi user chọn bài này. Không cần push code.

## 15–30 phút: COMMUNICATE (Giao tiếp)

**1. Liên hệ trực tiếp khách hàng (Qua DM Facebook/Email):**
```text
Chào [Tên KH],

Đây là [Tên Founder], Founder của AI Gym Coach. Mình vừa xem video bạn đăng. 

Việc xảy ra: Hệ thống AI của bên mình đã bắt sai góc lưng trong video của bạn và đưa ra phản hồi không chuẩn xác. Điều này rất nguy hiểm và mình hoàn toàn nhận lỗi.
Mình đang làm gì: Mình đã tạm khóa tính năng chấm Deadlift trên toàn hệ thống để rà soát lại thuật toán. 
Bồi thường: Mình xin hoàn lại 100% gói Pro của bạn (199K) ngay hôm nay (không cần điền form gì cả). Nếu bạn có bất kỳ vấn đề gì về sức khỏe sau cú lift đó, xin hãy báo mình biết.
Mình sẽ gọi cho bạn trong 24h tới để cập nhật tình hình sửa lỗi thuật toán: [SĐT trực tiếp của Founder]

-- [Tên Founder]
[founder@aigymcoach.vn]
```

**2. Đăng thông báo Public (Dưới bài post của khách / Twitter):**
```text
Hi mọi người — mình là Founder AI Gym Coach. Mình vừa check database hệ thống, AI của mình đã nhận diện sai góc lưng trong clip này.
Mình đã tạm khóa riêng bài Deadlift trên app để fix lỗi ngay. Các bài khác vẫn hoạt động bình thường. Mình sẽ update lại trong 24h tới.
Mình đang liên hệ trực tiếp với chủ post để xử lý.
— [Tên Founder]
```
