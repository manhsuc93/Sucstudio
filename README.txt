LIVESTREAM STUDIO - PWA

CÁCH ĐƯA LÊN GITHUB
1. Giải nén file ZIP.
2. Mở repository Livestreamapp trên GitHub.
3. Chọn Add file > Upload files.
4. Tải TOÀN BỘ nội dung bên trong thư mục livestreamapp_pwa lên thư mục gốc repository (không tải nguyên thư mục lồng bên trong).
5. Chọn Commit changes.
6. Đợi GitHub Pages cập nhật rồi mở lại URL ứng dụng bằng HTTPS.
7. Dán URL trang chủ vào PWABuilder và bấm Test lại.

CÁC FILE QUAN TRỌNG
- index.html: giao diện ứng dụng.
- manifest.json: tên, mô tả, màu, hướng màn hình, ID, icons, screenshots.
- sw.js: Service Worker để hỗ trợ cache/offline.
- icons/: biểu tượng PWA.
- screenshots/: ảnh minh họa cho manifest.

LƯU Ý
- Nếu repository đang dùng nhánh/folder khác cho GitHub Pages, hãy tải các file vào đúng thư mục đang được publish.
- Sau khi cập nhật, nếu PWABuilder vẫn báo cũ, kiểm tra URL manifest.json có mở được trực tiếp không và bấm Test lại.
- Bản này có xem trước camera và overlay cơ bản. Nút BẮT ĐẦU PHÁT chỉ bật trạng thái xem trước, chưa phát RTMP thật. Không đưa Stream Key thật vào mã nguồn công khai.
