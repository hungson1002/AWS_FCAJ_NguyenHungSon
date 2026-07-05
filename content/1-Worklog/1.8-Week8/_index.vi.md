---
title: "Worklog Tuần 8"
date: 2026-06-27
weight: 8
chapter: false
pre: " <b> 1.8. </b> "
---

### Mục tiêu tuần 8:

* Nghiên cứu và triển khai tính năng Custom Shipyard cho phép người chơi tự thiết kế hình dáng tàu.
* Xây dựng hệ thống bảng xếp hạng (Leaderboard) hiển thị TOP 5 người chơi điểm số cao nhất theo từng bậc rank.
* Tối ưu hóa giao diện người dùng (UI) trên cả Desktop và Responsive di động.
* Tăng cường tính bảo mật và độ ổn định của hệ thống bằng cách cấu hình Throttling (Rate Limiting) trên API Gateway.

### Công việc thực hiện trong tuần:

| Ngày | Công việc | Ngày bắt đầu | Ngày hoàn thành |
| ---- | --------- | ------------ | --------------- |
| 2 | - Nghiên cứu thiết kế giải pháp Custom Shipyard và cấu trúc lưới tọa độ cho phép người dùng tự vẽ hình dáng tàu <br> - Định nghĩa các quy tắc kiểm tra tính hợp lệ của tàu tự thiết kế | 22/06/2026 | 22/06/2026 |
| 3 | - Hiện thực hóa giao diện Custom Shipyard trên Frontend với công cụ vẽ lưới trực quan <br> - Xây dựng thuật toán xác thực tàu hợp lệ (liền mạch, đúng số lượng ô) và lưu trữ cấu hình tàu tùy chỉnh của người chơi | 23/06/2026 | 23/06/2026 |
| 4 | - Thiết kế cấu trúc cơ sở dữ liệu DynamoDB và viết Lambda Function truy vấn thông tin bảng xếp hạng <br> - Thực hiện lấy dữ liệu TOP 5 người chơi có điểm số cao nhất của mọi bậc rank | 24/06/2026 | 24/06/2026 |
| 5 | - Tích hợp giao diện Leaderboard lên Frontend hiển thị bảng xếp hạng chi tiết <br> - Tối ưu hóa giao diện người dùng (UI) trên Desktop và responsive hoàn chỉnh trên thiết bị di động (Mobile/Tablet) | 25/06/2026 | 25/06/2026 |
| 6 | - Cấu hình tính năng Throttling (Rate Limiting) trên AWS API Gateway cho các endpoint dịch vụ <br> - Thiết lập giới hạn số lượng request tối đa trong một đơn vị thời gian để ngăn chặn hành vi spam API | 26/06/2026 | 26/06/2026 |
| 7 | - Tiến hành kiểm thử toàn diện các tính năng mới (Custom Shipyard, Leaderboard, API Throttling) <br> - Sửa các lỗi phát sinh liên quan đến giao diện responsive và hoàn thành báo cáo tiến độ tuần 8 | 27/06/2026 | 27/06/2026 |

### Kết quả đạt được:

* **Hoàn thành tính năng Custom Shipyard sáng tạo:**
  * Cho phép người chơi tự do vẽ và thiết kế hình dạng tàu chiến theo sở thích cá nhân, không bị gò bó bởi các khuôn mẫu cố định.
  * Tích hợp thành công các thuật toán xác thực tính hợp lệ của tàu vẽ tay (đảm bảo các ô tàu liền mạch, đủ kích thước quy định) trước khi bắt đầu trận đấu.

* **Hoàn thiện hệ thống Bảng xếp hạng (Leaderboard):**
  * Triển khai thành công API truy vấn dữ liệu từ DynamoDB để trả về TOP 5 người chơi xuất sắc nhất sở hữu điểm số cao nhất của mọi bậc rank (Bronze, Silver, Gold, Platinum, Diamond, v.v.).
  * Thiết kế giao diện bảng xếp hạng trực quan, bắt mắt, tích hợp huy hiệu rank và thống kê tỉ số chi tiết.

* **Cải thiện trải nghiệm giao diện (UI) và Responsive:**
  * Tối ưu hóa bố cục giao diện hiển thị chuyên nghiệp trên màn hình Desktop, tăng kích thước các khu vực tương tác và điều khiển.
  * Cải tiến responsive toàn diện, loại bỏ hoàn toàn các lỗi bể layout, lệch phần tử trên điện thoại và máy tính bảng.

* **Bảo mật và chống Spam với API Gateway Throttling:**
  * Cấu hình thành công cơ chế Throttling trên AWS API Gateway (giới hạn Rate và Burst request).
  * Giúp bảo vệ tài nguyên hệ thống, ngăn chặn các cuộc tấn công spam API liên tục từ người dùng xấu, giảm thiểu chi phí phát sinh ngoài ý muốn từ AWS Lambda.
