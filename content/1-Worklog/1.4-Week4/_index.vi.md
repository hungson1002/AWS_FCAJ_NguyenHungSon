---
title: "Worklog Tuần 4"
date: 2024-01-01
weight: 4
chapter: false
pre: " <b> 1.4. </b> "
---
### Mục tiêu tuần 4:

* Tìm hiểu các kiến thức cơ bản về mạng trên AWS thông qua Amazon VPC.
* Thực hành cấu hình Security Group để kiểm soát lưu lượng truy cập vào tài nguyên AWS.
* Triển khai và quản lý Amazon EC2 Instance.
* Tìm hiểu cách tổ chức tài nguyên AWS bằng Tag.
* Thực hành tích hợp AWS với Slack thông qua Webhook.
* Thảo luận và lựa chọn đề tài cho dự án cuối kỳ thực tập.

### Công việc thực hiện trong tuần:

| Ngày | Công việc                                                                                                                                   | Ngày bắt đầu | Ngày hoàn thành |
| ---- | ------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- |
| 2    | - Tìm hiểu kiến thức về Amazon VPC <br> - Tạo và cấu hình VPC <br> - Nghiên cứu Subnet, Route Table và Internet Gateway                     | 25/05/2026   | 25/05/2026      |
| 3    | - Tìm hiểu Security Group <br> - Cấu hình các quy tắc Inbound và Outbound <br> - Tìm hiểu các nguyên tắc bảo mật mạng trên AWS              | 26/05/2026   | 26/05/2026      |
| 4    | - Khởi tạo Amazon EC2 Instance <br> - Cấu hình và kết nối EC2 <br> - Tìm hiểu quy trình triển khai máy chủ trên AWS                         | 27/05/2026   | 28/05/2026      |
| 5    | - Tìm hiểu AWS Tagging <br> - Tạo và quản lý Tag cho tài nguyên AWS <br> - Tìm hiểu cách quản lý và phân loại tài nguyên                    | 29/05/2026   | 29/05/2026      |
| 6    | - Tích hợp Slack Webhook với AWS <br> - Cấu hình gửi thông báo từ AWS đến Slack <br> - Tìm hiểu cơ chế giám sát và cảnh báo                 | 30/05/2026   | 30/05/2026      |
| 7    | - Thảo luận các ý tưởng cho dự án cuối kỳ <br> - Lựa chọn đề tài Battleship Game <br> - Nghiên cứu các dịch vụ AWS có thể áp dụng cho dự án | 31/05/2026   | 31/05/2026      |

### Kết quả đạt được:

* Tạo thành công môi trường mạng riêng trên AWS bằng Amazon VPC.

* Hiểu được các thành phần quan trọng trong hệ thống mạng AWS:

  * Virtual Private Cloud (VPC)
  * Subnet
  * Route Table
  * Internet Gateway
  * CIDR Block

* Cấu hình thành công Security Group và hiểu được cơ chế kiểm soát lưu lượng mạng của AWS.

* Thực hành triển khai và quản lý Amazon EC2 Instance.

* Hiểu được các thành phần quan trọng trong quá trình khởi tạo EC2:

  * Amazon Machine Image (AMI)
  * Instance Type
  * Security Group
  * Network Configuration

* Tìm hiểu và thực hành gắn Tag cho các tài nguyên AWS.

* Hiểu được vai trò của Tag trong:

  * Quản lý tài nguyên
  * Theo dõi chi phí
  * Phân loại môi trường
  * Quản lý dự án

* Tích hợp thành công AWS với Slack thông qua Webhook.

* Hiểu được cách các dịch vụ AWS có thể gửi thông báo đến các nền tảng cộng tác bên ngoài để phục vụ mục đích giám sát và cảnh báo.

* Bắt đầu giai đoạn lên ý tưởng cho dự án cuối kỳ thực tập.

* Thống nhất lựa chọn đề tài:

  * Battleship Game

* Xây dựng định hướng kiến trúc ban đầu cho dự án với các dịch vụ AWS dự kiến sử dụng:

  * Amazon Cognito
  * AWS Lambda
  * Amazon S3
  * Amazon API Gateway
  * Amazon DynamoDB
  * Amazon CloudFront

* Thảo luận về:

  * Cơ chế đăng nhập và xác thực người dùng
  * Luồng xử lý trò chơi
  * Kiến trúc hệ thống trên nền tảng AWS
  * Khả năng triển khai theo mô hình Cloud-Native

* Hoàn thành quá trình học tập và thực hành các nội dung từ video 150 - 181 thuộc chương trình First Cloud Journey Bootcamp 2025 của AWS Study Group.

---

## Nguyên Hùng Sơn

### Mục tiêu tuần 4:

* Thực hành giới hạn quyền hạn IAM Policy, cấu hình Switch Role ràng buộc điều kiện và gán IAM Role cho EC2.
* Thực hành bảo vệ và giám sát dữ liệu: mã hoá S3 bằng KMS, ghi nhật ký bằng CloudTrail và truy vấn log qua Amazon Athena.
* Tìm hiểu lý thuyết về cơ sở dữ liệu trên AWS gồm RDS, Aurora, Redshift và ElastiCache.
* Thực hành triển khai ứng dụng Multi-Tier kết nối EC2 và RDS trong mạng VPC tùy chỉnh.
* Thảo luận và lập kế hoạch lựa chọn dịch vụ AWS cho dự án tốt nghiệp cuối khóa.

### Công việc thực hiện trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Thực hành giới hạn quyền IAM: tạo chính sách giới hạn quyền hạn (Restriction Policy) và kiểm tra truy cập của IAM User bị giới hạn | 25/05/2026 | 25/05/2026 | https://www.youtube.com/playlist?list=PLahN4TLWtox2a3vElknwzU_urND8hLn1i |
| 3 | - Thực hành giám sát bảo mật: cấu hình mã hóa KMS cho S3 bucket, tạo trail trên CloudTrail và thực hiện truy vấn nhật ký bằng Amazon Athena | 26/05/2026 | 26/05/2026 | https://www.youtube.com/playlist?list=PLahN4TLWtox2a3vElknwzU_urND8hLn1i |
| 4 | - Thực hành cấu hình Switch Role: thiết lập vai trò chuyển đổi và tạo các điều kiện giới hạn theo IP nguồn, thời gian | 27/05/2026 | 27/05/2026 | https://www.youtube.com/playlist?list=PLahN4TLWtox2a3vElknwzU_urND8hLn1i |
| 5 | - Thực hành bảo mật thông tin xác thực EC2: so sánh việc sử dụng Access Key với việc gắn IAM Role trực tiếp cho EC2 khi truy cập S3 | 28/05/2026 | 28/05/2026 | https://www.youtube.com/playlist?list=PLahN4TLWtox2a3vElknwzU_urND8hLn1i |
| 6 | - Tìm hiểu lý thuyết cơ sở dữ liệu trên AWS: RDS, Aurora, Redshift và ElastiCache | 29/05/2026 | 30/05/2026 | https://www.youtube.com/playlist?list=PLahN4TLWtox2a3vElknwzU_urND8hLn1i |
| 7 | - Thực hành lab RDS: xây dựng mạng multi-tier VPC, tạo subnet group, khởi tạo EC2 app server và RDS instance, chạy thử ứng dụng và kiểm tra backup/restore database <br> - Thảo luận nhóm lựa chọn đề tài Battleship Game làm dự án tốt nghiệp | 31/05/2026 | 31/05/2026 | Thảo luận nhóm & Tài liệu AWS |

### Kết quả đạt được tuần 4:

* Thực hành thiết lập chính sách giới hạn quyền hạn và kiểm thử thành công các giới hạn truy cập đối với IAM User.

* Thực hành mã hóa và ghi nhật ký hoạt động: tạo khóa KMS mã hóa S3 bucket, kích hoạt CloudTrail và sử dụng Amazon Athena truy vấn lịch sử hoạt động.

* Thực hành Switch Role kèm ràng buộc điều kiện theo địa chỉ IP nguồn và khung giờ truy cập cụ thể.

* Thực hành quản lý thông tin xác thực: gán IAM Role cho EC2 instance thay thế việc lưu trữ thủ công Access Key.

* Theo dõi các video tổng quan về các dịch vụ cơ sở dữ liệu AWS: RDS, Aurora, Redshift và ElastiCache.

* Thực hành triển khai ứng dụng Multi-Tier: thiết lập EC2 liên kết với RDS database trong mạng VPC và kiểm tra tính năng backup/restore cơ sở dữ liệu.

* Tham gia thảo luận nhóm hoạch định kiến trúc dự án cuối kỳ: thống nhất chọn đề tài Battleship Game và thảo luận các dịch vụ AWS liên quan (Cognito, Lambda, S3, API Gateway, DynamoDB, CloudFront).

* Đã hoàn thành theo dõi và học các video từ 187 - 228 trong series First Cloud Journey Bootcamp 2025 của AWS Study Group.

