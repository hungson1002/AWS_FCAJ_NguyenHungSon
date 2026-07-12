---
title: "Worklog Tuần 4"
date: 2024-01-01
weight: 4
chapter: false
pre: " <b> 1.4. </b> "
---


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

