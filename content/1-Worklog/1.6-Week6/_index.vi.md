---
title: "Worklog Tuần 6"
date: 2026-06-14
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
---

### Mục tiêu tuần 6:

* Kết nối và tích hợp dịch vụ xác thực người dùng AWS Cognito cho ứng dụng.
* Hoàn thiện thiết kế Responsive (tương thích đa thiết bị) cho toàn bộ giao diện của ứng dụng Battleship Game.
* Xây dựng kiến trúc Serverless sử dụng AWS Lambda và Amazon API Gateway để xử lý logic backend và gọi các API.
* Thiết lập Amazon DynamoDB để lưu trữ thông tin người dùng và dữ liệu lịch sử trận đấu.
* Nghiên cứu giải pháp và lên kế hoạch chi tiết cho việc phát triển chế độ PvP thời gian thực bằng giao thức WebSocket.

### Công việc thực hiện trong tuần:

| Ngày | Công việc | Ngày bắt đầu | Ngày hoàn thành |
| ---- | --------- | ------------ | --------------- |
| 2 | - Tìm hiểu và cấu hình Amazon Cognito User Pool và Client App <br> - Tích hợp AWS Cognito SDK vào ứng dụng React (Frontend) để quản lý luồng đăng ký, đăng nhập, đăng xuất và trạng thái phiên làm việc của người dùng | 08/06/2026 | 08/06/2026 |
| 3 | - Thiết kế và hoàn thiện tính tương thích Responsive cho giao diện game <br> - Tối ưu hóa bố cục, menu điều hướng và kích thước bảng tàu trên các thiết bị di động/máy tính bảng <br> - Sửa đổi luồng khởi tạo âm thanh và tối ưu hóa sự kiện lắng nghe kích thước màn hình để tránh làm nghẽn luồng xử lý chính khi chuyển hướng từ Cognito | 09/06/2026 | 09/06/2026 |
| 4 | - Thiết kế kiến trúc Serverless cho ứng dụng Battleship <br> - Cấu hình Amazon API Gateway kết nối với AWS Lambda <br> - Tạo và triển khai Lambda Function `getUser` để xử lý việc lấy thông tin chi tiết hồ sơ người chơi | 10/06/2026 | 10/06/2026 |
| 5 | - Nghiên cứu thiết kế cơ sở dữ liệu NoSQL với Amazon DynamoDB <br> - Định nghĩa cấu trúc bảng lịch sử trận đấu, xác định Partition Key (userId) và Sort Key (endedAt) để truy vấn hiệu quả <br> - Tạo Lambda Function `saveMatchHistory` để ghi nhận và lưu kết quả trận đấu sau khi trò chơi kết thúc | 11/06/2026 | 11/06/2026 |
| 6 | - Tích hợp API gọi lưu kết quả trận đấu (`savePvpRankedMatchHistory`) và lấy thông tin thống kê người dùng lên giao diện Profile <br> - Hoàn thiện Lambda Function hiển thị lịch sử đấu có phân trang (pagination) và ẩn mã phòng chơi (room code masking) vì lý do bảo mật | 12/06/2026 | 12/06/2026 |
| 7 | - Tìm hiểu cơ chế truyền thông điệp thời gian thực qua giao thức WebSocket <br> - Lên phương án kiến trúc cho chế độ chơi mạng PvP (tạo phòng chơi, tham gia phòng chơi, đồng bộ hóa tọa độ bắn tàu và trạng thái lượt đi của 2 người chơi) | 13/06/2026 | 14/06/2026 |

### Kết quả đạt được:

* **Tích hợp thành công Xác thực người dùng (AWS Cognito):**
  * Đã cấu hình và kết nối thành công ứng dụng Frontend với Cognito User Pool.
  * Triển khai hoàn tất luồng đăng ký tài khoản, xác thực mã OTP gửi về email, đăng nhập và đăng xuất.
  * Tối ưu hóa luồng xử lý sau khi chuyển hướng đăng nhập (OAuth redirect flow) trên Single Page Application (SPA), khắc phục triệt để hiện tượng treo giao diện trên các thiết bị di động.

* **Hoàn thiện thiết kế Responsive và UI/UX:**
  * Bố cục giao diện co giãn linh hoạt trên mọi loại màn hình (Mobile, Tablet, Desktop) nhờ tận dụng Flexbox, CSS Grid và Media Queries.
  * Tối ưu hóa trải nghiệm điều hướng bằng cách triển khai thanh điều hướng thu gọn dạng Hamburger trên thiết bị di động.
  * Giải quyết dứt điểm các lỗi phát sinh do sự kiện thay đổi kích thước trình duyệt và khởi tạo tài nguyên âm thanh (Audio API) gây hao tổn tài nguyên và treo trình duyệt.

* **Xây dựng hệ thống Serverless API & Routing:**
  * Tạo thành công API Gateway làm điểm đầu nhận yêu cầu (REST API) tích hợp CORS.
  * Triển khai các hàm Lambda xử lý nghiệp vụ với hiệu năng cao, cơ chế phân quyền bảo mật thông qua IAM Roles.
  * Thiết lập xác thực tự động các yêu cầu API bằng JSON Web Tokens (JWT) tạo bởi Cognito để bảo vệ tài nguyên hệ thống.

* **Lưu trữ dữ liệu tối ưu với DynamoDB:**
  * Thiết kế bảng DynamoDB có hiệu năng cao với cấu trúc khóa thông minh: Partition Key (`userId`) và Sort Key (`endedAt`), giúp truy vấn lịch sử đấu của người dùng cực kỳ nhanh chóng và tiết kiệm chi phí đọc/ghi.
  * Đồng bộ thành công số liệu thống kê (Số trận đã chơi, Số trận thắng, Số trận thua, Tỉ lệ bắn chính xác) hiển thị trực tiếp từ cơ sở dữ liệu lên giao diện Profile người chơi.
  * Triển khai tính năng ẩn thông tin phòng đấu nhạy cảm khi hiển thị dữ liệu lịch sử trận đấu công khai.

* **Lên kế hoạch phát triển PvP thời gian thực bằng WebSocket:**
  * Hoàn thành tài liệu thiết kế kiến trúc phòng chờ (Lobby) và phòng đấu (Match Room) sử dụng API Gateway WebSocket APIs.
  * Xác định cấu trúc gói tin JSON truyền nhận giữa 2 người chơi và luồng logic đồng bộ hóa từng lượt đi (turn-based state synchronization).
