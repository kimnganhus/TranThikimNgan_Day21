# AI Safety Rules — v1
[Last updated: 07/05/2026 by Founder]

## Tình huống rủi ro lớn nhất (Top Risk)
Admin hoặc Kỹ sư backend truy cập vào Supabase Storage (bucket `videos-raw`) tự ý tải video khách hàng đang tập gym về máy cá nhân, hoặc upload lên các công cụ public (như ChatGPT/Claude) để debug lỗi phân tích của MediaPipe, dẫn đến vi phạm quyền riêng tư nghiêm trọng (PDPA/GDPR).

## 1. RULES (Luật)
- ❌ **Cấm cụ thể:** KHÔNG tải video từ bucket `videos-raw` của khách hàng về máy tính cá nhân. KHÔNG upload video hoặc frame hình của khách lên ChatGPT/Claude bản public để nhờ AI debug code MediaPipe.
- ✅ **Được làm:** 
  1. Chỉ debug dựa trên JSON payload lỗi và metadata.
  2. Nếu bắt buộc phải dùng AI để đọc lỗi, DÙNG **Claude for Work** (đã được công ty cấp tài khoản, cam kết không dùng data để train model, cost: $30/user/tháng). Link login: [Link nội bộ]
- ⚠️ **Hậu quả vi phạm:** Lần 1: Founder gọi nói chuyện 1-1, ghi note cảnh cáo và cắt ngay quyền truy cập Dashboard Supabase. Lần 2: Buộc thôi việc (Let go).
- 🔄 **Cập nhật:** Founder trực tiếp cập nhật rule này qua [Notion Link] mỗi khi có thay đổi.

## 2. RAILS (Rào chắn công nghệ)
Ngân sách dự kiến: $150/tháng (Thỏa mãn tiêu chí startup <$500/tháng)
- **Tool 1 (Chặn hành vi bằng Code):** Viết RLS (Row Level Security) policy trên Supabase chặn hoàn toàn quyền đọc/tải từ giao diện UI Admin của file trong `videos-raw`. Cập nhật Cronjob tự động xóa `videos-raw` sau **24 giờ** (thay vì 30 ngày như PRD cũ). (Cost: **$0/tháng**, cấu hình trên hạ tầng Supabase có sẵn).
- **Tool 2 (Giải pháp thay thế an toàn):** Cấp tài khoản **Claude for Team** cho team dev (5 engineers) để phân tích log/code mà không sợ rò rỉ dữ liệu lên model công cộng. (Cost: ~$30/user/tháng x 5 = **$150/tháng**).

## 3. RITUAL (Nghi thức văn hóa)
- **Tên Ritual:** Friday 30' Privacy & ML Debug Review
- **Tần suất:** Chiều thứ 6 hàng tuần (30 phút).
- **Hành động:** Founder và Tech Lead ngồi lại review các lỗi phát sinh từ ML Service (Railway).
- **Câu hỏi cụ thể Founder sẽ hỏi Tech Lead:** *"Tuần này có bao nhiêu job MediaPipe bị timeout hoặc fail? Mọi người đã dùng cách nào để fix lỗi góc quay (TIP-058c) mà không cần tải file raw của khách về máy? Có ai gặp khó khăn khi dùng Claude Enterprise không?"*
