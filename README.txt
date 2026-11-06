# Live Studio V15 — iPhone + WHIP/WebRTC Multi-stream

## Ứng dụng
- Mở `Studio_Livestream.html` hoặc trang GitHub Pages (index tự chuyển tới studio).
- Dùng Safari trên iPhone qua HTTPS; cấp quyền Camera và Micro.
- Vùng xem trước camera hiển thị camera; chương trình gửi đi được ghép từ camera/chia sẻ màn hình + overlay canvas.
- Chuyển luồng đến máy chủ bằng WHIP/WebRTC. Nút BẮT ĐẦU PHÁT thực hiện POST WHIP; nút KẾT THÚC PHÁT đóng phiên WHIP.
- Chọn 1080p30 (4.5 Mbps) hoặc 720p30 (2.5 Mbps). iPhone và mạng di động có thể không duy trì được 1080p ổn định.

## Phát đồng thời YouTube + Facebook
Cần triển khai máy chủ MediaMTX + FFmpeg + Caddy trong thư mục `server/`. Đọc `server/README-VI.txt` để cài đặt từng bước.
- Live Studio gửi một luồng WebRTC/WHIP đến VPS.
- VPS chuyển âm thanh Opus sang AAC rồi gửi RTMPS tới YouTube và Facebook.
- Khóa phát YouTube/Facebook và mật khẩu relay phải chỉ lưu trên VPS. Không đưa khóa thật vào GitHub.
- Trong Live Studio > Cài đặt, nhập `https://TEN-MIEN-CUA-BAN/live/whip`, tài khoản `studio` và mật khẩu VPS.

## Kiểm tra trước khi phát
1. Bật Camera.
2. Bật Micro (bắt buộc vì nền tảng cần cả audio và video).
3. Cấu hình VPS, DNS, HTTPS và cổng UDP/TCP 8189.
4. Nhấn BẮT ĐẦU PHÁT; chờ trạng thái WebRTC kết nối.
5. Kiểm tra bản xem trước trong YouTube Studio và Facebook Live Producer; nhấn Go Live trên từng nền tảng nếu được yêu cầu.

## Giới hạn
- Chưa thể xác nhận hoạt động thật cho đến khi có VPS, domain, khóa phát và kiểm tra trên iPhone thực tế.
- Nút “Kiểm tra cấu hình WHIP” chỉ kiểm tra định dạng; không thay cho kết nối thật.
- Nếu mạng chặn UDP/TCP 8189 hoặc NAT khó, có thể cần TURN server (Coturn).
- Chức năng chia sẻ màn hình trên iOS bị giới hạn theo phiên bản Safari/iOS.
