# Live Studio — Mobile PWA

Bản nâng cấp từ mã nguồn ban đầu, hướng tới iPhone và Android.

## Cài đặt / chạy thử
1. Đưa toàn bộ thư mục lên hosting có HTTPS (GitHub Pages, Netlify hoặc hosting tương đương).
2. Mở `Studio_Livestream.html` hoặc truy cập thư mục gốc (tự chuyển tới studio).
3. Cấp quyền camera/micro. Trên iPhone dùng Safari; trên Android dùng Chrome phiên bản mới.
4. Có thể thêm ứng dụng vào màn hình chính từ menu trình duyệt.

## Đã nâng cấp
- PWA mở thống nhất vào `Studio_Livestream.html`; manifest, scope và service worker dùng chung.
- Giao diện có safe-area và bố cục thích ứng màn hình điện thoại.
- Camera có ràng buộc độ phân giải/FPS hợp lý, đổi thiết bị, thông báo lỗi quyền và giải phóng track.
- Micro có khử vọng/giảm ồn, gain thực, đồng hồ mức âm thanh và giải phóng stream/AudioContext khi tắt.
- Chia sẻ màn hình có kiểm tra hỗ trợ và xử lý khi người dùng dừng chia sẻ. Tính năng này bị giới hạn trên một số trình duyệt di động, đặc biệt iOS.
- Canvas dựng hình giới hạn khoảng 30 FPS để giảm tải; logo giới hạn 5 MB.
- Tin nhắn người dùng được đưa vào DOM bằng `textContent` để tránh XSS.
- Không nhúng Stream Key mẫu trong mã nguồn.

## Giới hạn cần hiểu rõ
Bản này là **studio xem trước / dựng hình tại trình duyệt**, chưa phải bộ phát RTMP hoàn chỉnh. Nút phát không giả vờ báo LIVE. `getProgramStream()` cung cấp MediaStream đã ghép hình Canvas và track micro để tích hợp encoder/gateway sau này.

Để phát thật lên YouTube/Facebook/RTMP cần một trong các phương án: (a) ứng dụng native với encoder phần cứng và dịch vụ RTMP; hoặc (b) backend WebRTC ingest/gateway chuyển tiếp sang RTMP (ví dụ MediaMTX/FFmpeg) cùng xác thực an toàn. Trình duyệt không thể gửi RTMP trực tiếp chỉ bằng URL và Stream Key. Không đưa Stream Key thật vào GitHub hoặc localStorage.

## Kiểm thử khuyến nghị
- iPhone Safari và Android Chrome qua HTTPS.
- Camera trước/sau, từ chối quyền, camera đang được ứng dụng khác dùng.
- Bật/tắt mic nhiều lần, kiểm tra đèn mic và meter.
- Dừng chia sẻ màn hình từ thanh hệ thống.
- Xoay màn hình, chuyển nền rồi quay lại.
- Kiểm tra PWA cập nhật sau khi tăng `CACHE_NAME` trong `sw.js`.

## Lưu ý bảo mật
Không công khai Stream Key, mật khẩu hoặc thông tin truy cập máy chủ. Dùng backend để lưu bí mật và cấp token ngắn hạn cho client.


## Sửa lỗi camera iPhone (v4)
- Video nguồn camera không còn dùng `display:none`; Safari iOS cần phần tử video có thể được giải mã để Canvas đọc được khung hình.
- Chờ `loadedmetadata`, gọi `video.play()` và xác nhận `videoWidth/videoHeight` trước khi báo camera hoạt động.
- Đổi cache Service Worker sang v4 để trình duyệt nhận bản cập nhật.
