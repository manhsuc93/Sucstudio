SUCSTUDIO V15 - MÁY CHỦ PHÁT ĐỒNG THỜI YOUTUBE + FACEBOOK
==========================================================

KIẾN TRÚC
- iPhone Safari gửi hình/tiếng bằng WebRTC WHIP tới VPS.
- MediaMTX nhận luồng; FFmpeg đổi âm thanh Opus sang AAC.
- VPS gửi cùng một chương trình tới YouTube và Facebook qua RTMPS.
- Khóa YouTube/Facebook chỉ nằm trong mediamtx.yml trên VPS, không nằm trong GitHub Pages.

CẦN CHUẨN BỊ
1. Một VPS Ubuntu 22.04/24.04, khuyến nghị tối thiểu 4 vCPU, 4 GB RAM, mạng công cộng ổn định.
   (FFmpeg trong gói này chỉ chuyển mã âm thanh AAC; video được copy, không mã hóa lại.)
2. Một tên miền hoặc tên miền phụ, ví dụ stream.tenmiencuaban.vn.
3. DNS A record của tên miền phụ trỏ tới IPv4 công cộng của VPS.
4. Khóa phát YouTube và Facebook lấy từ trang quản lý phát trực tiếp của từng nền tảng.

BƯỚC 1 - CHUẨN BỊ VPS
- Đăng nhập VPS bằng SSH.
- Cài Docker Engine và Docker Compose Plugin theo hướng dẫn chính thức của Docker.
- Mở firewall/security group các cổng:
    TCP 80, TCP 443, UDP 8189, TCP 8189.
- Không mở công khai cổng RTSP 8554 hoặc WHIP 8889; Caddy sẽ proxy HTTPS tới MediaMTX.

BƯỚC 2 - ĐƯA THƯ MỤC SERVER LÊN VPS
- Sao chép toàn bộ thư mục server này vào VPS, ví dụ /opt/sucstudio.
- Vào thư mục: cd /opt/sucstudio
- Sửa Caddyfile: thay stream.YOUR-DOMAIN.vn bằng tên miền thật.
- Sửa mediamtx.yml:
  * stream.YOUR-DOMAIN.vn: thay bằng cùng tên miền thật ở 2 vị trí.
  * CHANGE_ME_STUDIO_PASSWORD: mật khẩu dài, ngẫu nhiên để nhập trong Live Studio.
  * CHANGE_ME_RELAY_PASSWORD: mật khẩu dài chỉ dùng nội bộ; chỉ dùng chữ và số để tránh lỗi URL.
  * CHANGE_ME_YOUTUBE_STREAM_KEY: khóa phát YouTube.
  * CHANGE_ME_FACEBOOK_STREAM_KEY: khóa phát Facebook.
- Không gửi các khóa hoặc mật khẩu này cho người khác và không đưa chúng vào GitHub.

BƯỚC 3 - LẤY KHÓA PHÁT
YouTube:
- Mở YouTube Studio > Create > Go live.
- Chọn luồng hoặc tạo luồng mới; sao chép stream key.
- Dùng RTMPS ingest mặc định: rtmps://a.rtmps.youtube.com/live2/

Facebook:
- Mở Live Producer: https://www.facebook.com/live/producer
- Chọn nơi đăng (trang cá nhân/Page) và Streaming software.
- Sao chép Server URL và Stream Key Facebook hiển thị. Nếu có tùy chọn Persistent stream key, bật nếu muốn dùng lại.
- URL mặc định thường là rtmps://live-api-s.facebook.com:443/rtmp/; nếu Live Producer đưa URL khác, dùng URL đang hiển thị và cập nhật trong lệnh FFmpeg ở mediamtx.yml.
- Với khóa Facebook thay đổi theo từng sự kiện, cập nhật khóa trong mediamtx.yml trước mỗi buổi phát rồi khởi động lại dịch vụ.

BƯỚC 4 - KHỞI ĐỘNG
Trong /opt/sucstudio chạy:
    docker compose up -d
    docker compose logs -f

Lần đầu, Caddy cần lấy chứng chỉ HTTPS tự động. Đảm bảo DNS đã trỏ đúng và cổng TCP 80/443 không bị chặn.
Thoát xem log bằng Ctrl+C; dịch vụ vẫn chạy nền.

BƯỚC 5 - CẤU HÌNH LIVE STUDIO V15
- Mở trang Live Studio trên GitHub Pages.
- Chọn tab Cài đặt.
- Địa chỉ WHIP: https://stream.tenmiencuaban.vn/live/whip
- Tên đăng nhập: studio
- Mật khẩu: giá trị CHANGE_ME_STUDIO_PASSWORD đã đặt trên VPS.
- Bật Camera và Bật Mic. Chờ micro được cấp quyền.
- Nhấn Kiểm tra cấu hình WHIP, sau đó BẮT ĐẦU PHÁT.

BƯỚC 6 - HOÀN TẤT PHÁT TRÊN NỀN TẢNG
- Quay lại YouTube Studio và Facebook Live Producer.
- Chờ cả hai nơi nhận được tín hiệu xem trước.
- Nếu nền tảng yêu cầu xác nhận, nhấn Go Live/Phát trực tiếp tại từng nền tảng.
- Nhấn KẾT THÚC PHÁT trong Live Studio khi muốn ngừng gửi luồng.

LƯU Ý QUAN TRỌNG
- Nút “Kiểm tra cấu hình WHIP” chỉ kiểm tra định dạng các trường, không kiểm tra VPS.
- Nút BẮT ĐẦU PHÁT mới thực hiện kết nối WHIP thực tế.
- Một số tài khoản Facebook/Page có điều kiện riêng để được phát trực tiếp.
- Luồng gửi lên VPS là 1080p30 khoảng 4.5 Mbps hoặc 720p30 khoảng 2.5 Mbps; cần upload iPhone ổn định cao hơn bitrate này.
- VPS gửi hai bản ra ngoài; với 4.5 Mbps, cần ít nhất khoảng 9 Mbps băng thông outbound thực tế, nên có dư địa lớn hơn.
- Nếu WebRTC không kết nối, kiểm tra DNS, chứng chỉ HTTPS, UDP/TCP 8189 và webrtcAdditionalHosts.
- Không bao giờ chụp/gửi ảnh có lộ khóa phát hoặc mật khẩu.
