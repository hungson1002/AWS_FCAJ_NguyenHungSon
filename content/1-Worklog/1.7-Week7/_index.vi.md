---
title: "Worklog Tuần 7"
date: 2026-06-20
weight: 7
chapter: false
pre: " <b> 1.7. </b> "
---

### Mục tiêu tuần 7:

* Hoàn thiện kết nối thời gian thực cho chế độ chơi PvP bằng cách tích hợp API Gateway WebSocket.
* Hoàn thiện tính năng tải lên ảnh đại diện (avatar) của người dùng tích hợp dịch vụ lưu trữ AWS S3.
* Hoàn thiện trang Hồ sơ cá nhân (Profile) hiển thị đầy đủ thống kê và lịch sử đấu chi tiết của người chơi.
* Tích hợp hiệu ứng âm thanh (Sound Effects) sống động cho các tương tác trong game và xử lý tối ưu hóa luồng tải tài nguyên âm thanh.
* Hoàn thiện khả năng hiển thị tương thích Responsive cho toàn bộ các màn hình của trò chơi.
* Khắc phục các lỗi về Lambda và API Gateway, đồng thời tái cấu trúc giải pháp lưu lịch sử trận đấu từ phía Server để đảm bảo tính toàn vẹn dữ liệu.

### Công việc thực hiện trong tuần:

| Ngày | Công việc | Ngày bắt đầu | Ngày hoàn thành |
| ---- | --------- | ------------ | --------------- |
| 2 | - Nghiên cứu cấu hình API Gateway WebSocket APIs với các route mặc định (`$connect`, `$disconnect`) và route tùy chỉnh (`joinRoom`, `makeMove`) <br> - Phát triển Lambda kết nối quản lý Session và Connection ID để lưu vết người chơi trực tuyến | 15/06/2026 | 15/06/2026 |
| 3 | - Kết nối WebSocket ở Frontend và xây dựng logic đồng bộ hóa thời gian thực (real-time game state) <br> - Tích hợp bộ sưu tập hiệu ứng âm thanh (Sound Effects) như tiếng nổ, tiếng đạn trượt, nhạc nền game <br> - Khắc phục các lỗi không tự động phát âm thanh (autoplay policy) bằng cách khởi tạo an toàn thông qua tương tác người dùng | 16/06/2026 | 16/06/2026 |
| 4 | - Xây dựng tính năng tải lên avatar người dùng lên AWS S3 <br> - Viết Lambda Function tạo S3 Presigned URL để Frontend tải ảnh trực tiếp lên S3 Bucket mà không để lộ khóa bí mật <br> - Cấu hình phân quyền IAM Policy và CORS cho S3 Bucket để tải và hiển thị ảnh mượt mà | 17/06/2026 | 17/06/2026 |
| 5 | - Cải tiến trang Hồ sơ người dùng (Profile): kết nối hiển thị avatar động từ S3 và lưu thông tin cập nhật vào DynamoDB <br> - Hoàn thiện hiển thị chi tiết chỉ số người chơi (Service Record) và danh sách lịch sử trận đấu theo định dạng trực quan | 18/06/2026 | 18/06/2026 |
| 6 | - Kiểm thử, tìm lỗi và khắc phục các vấn đề liên quan đến API Gateway CORS và Lambda timeouts <br> - Thiết kế và triển khai giải pháp tối ưu bảo mật: chuyển đổi cơ chế lưu lịch sử trận đấu (`saveMatchHistory`) từ phía Client gọi API sang Backend tự động lưu trực tiếp khi nhận diện sự kiện kết thúc trận đấu qua WebSocket | 19/06/2026 | 19/06/2026 |
| 7 | - Tối ưu và hoàn thiện thiết kế Responsive cho toàn bộ các màn hình game (Lobby, bốc xếp tàu, bảng đấu súng, profile) <br> - Kiểm thử tính ổn định của WebSocket khi mất kết nối mạng và hoàn thành báo cáo tiến độ tuần 7 | 20/06/2026 | 21/06/2026 |

### Kết quả đạt được:

* **Hoàn thành chế độ PvP trực tuyến thời gian thực (WebSocket):**
  * Triển khai thành công API Gateway WebSocket kết hợp với AWS Lambda để quản lý kết nối và ghép phòng chơi trực tuyến.
  * Đồng bộ hóa tức thời các sự kiện trong trận đấu (lượt đi, tọa độ bắn, trạng thái tàu chìm) giúp trải nghiệm chơi PvP mượt mà và không có độ trễ lớn.

* **Hoàn thiện tính năng Tải ảnh đại diện với AWS S3:**
  * Triển khai giải pháp tải ảnh bảo mật cao thông qua cơ chế S3 Presigned URL, giảm tải cho server backend.
  * Tự động lưu và cập nhật đường dẫn ảnh đại diện mới vào DynamoDB để hiển thị đồng bộ trên toàn bộ hệ thống.

* **Hoàn thiện Hồ sơ cá nhân (Profile):**
  * Tích hợp thành công dữ liệu động từ backend hiển thị đầy đủ thông số: Tổng số trận, Tỷ lệ thắng/thua, Độ chính xác của các phát bắn (Shots/Hits/Misses).
  * Hiển thị bảng lịch sử đấu trực quan với đầy đủ thông tin (avatar đối thủ, chế độ chơi rank/casual, kết quả thắng/thua, thời gian thi đấu).

* **Nâng cao Trải nghiệm âm thanh (Sound Effects):**
  * Thêm hiệu ứng âm thanh phong phú cho từng sự kiện: Bắn trúng tàu (Hit), Bắn trượt (Miss), Tàu bị phá hủy (Sunk), và Kết thúc trận đấu (Win/Loss).
  * Xử lý tối ưu lỗi bất đồng bộ khi tải audio giúp trang web chạy nhanh hơn và tránh hiện tượng giật lag trên các thiết bị di động.

* **Tối ưu hóa Responsive toàn diện:**
  * Toàn bộ trò chơi hiển thị hoàn hảo trên các thiết bị di động và máy tính bảng nhờ thiết kế CSS thích ứng linh hoạt.
  * Khắc phục triệt để lỗi căn lề, tràn màn hình và hiển thị lưới tàu (Grid) bị méo trên các độ phân giải màn hình nhỏ.

* **Tối ưu hóa Bảo mật và Sửa lỗi Hệ thống:**
  * Sửa hoàn toàn các lỗi liên quan đến tích hợp API Gateway (lỗi CORS, lỗi cấu trúc Request/Response JSON) và Lambda timeout.
  * **Giải pháp tối ưu hóa dữ liệu:** Loại bỏ hoàn toàn lỗ hổng bảo mật khi cho phép Client gửi yêu cầu API để lưu kết quả trận đấu. Hiện tại, server (qua WebSocket Backend) sẽ trực tiếp xác thực kết quả trận đấu từ trạng thái game và tự động ghi dữ liệu vào DynamoDB, ngăn chặn hành vi gian lận sửa đổi điểm số từ phía Client.
