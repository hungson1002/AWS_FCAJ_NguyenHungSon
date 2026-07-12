---

title: "Worklog Tuần 3"
date: 2026-05-24
weight: 3
chapter: false
pre: " <b> 1.3. </b> "
---

### Mục tiêu tuần 3:

* Thực hành AWS Storage Gateway và Amazon FSx (SSD/HDD Multi-AZ).
* Thực hành Amazon S3 Static Website Hosting, CloudFront, S3 Versioning và Replication đa vùng.
* Tìm hiểu lý thuyết về Bảo mật & Định danh trên AWS: IAM, Cognito, AWS Organizations, Identity Center và KMS.
* Thực hành tự động hoá thao tác trên EC2 bằng AWS Lambda và gửi cảnh báo qua Slack Incoming Webhooks.
* Tìm hiểu cách quản lý tag tài nguyên, Resource Groups và cấu hình phân quyền IAM dựa trên tag (Tag-based access control).

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Thực hành Storage Gateway: tạo Storage Gateway, File Share và mount File Share vào máy On-premises <br> - Thực hành FSx: tạo SSD/HDD Multi-AZ file system, cấu hình quotas, shadow copies, co giãn throughput/storage | 18/05/2026 | 18/05/2026 | https://www.youtube.com/playlist?list=PLahN4TLWtox2a3vElknwzU_urND8hLn1i |
| 3 | - Thực hành Amazon S3 & CloudFront: cấu hình Static Website, chặn public access và tích hợp CloudFront CDN, cấu hình Versioning, di chuyển object, Replication đa vùng | 19/05/2026 | 20/05/2026 | https://www.youtube.com/playlist?list=PLahN4TLWtox2a3vElknwzU_urND8hLn1i |
| 4 | - Tìm hiểu lý thuyết Bảo mật & Định danh: Mô hình trách nhiệm chung, IAM, Cognito, AWS Organizations, Identity Center, KMS <br> - Thực hành Security Hub: kích hoạt và theo dõi điểm tiêu chuẩn bảo mật | 21/05/2026 | 21/05/2026 | https://www.youtube.com/playlist?list=PLahN4TLWtox2a3vElknwzU_urND8hLn1i |
| 5 | - Thực hành tích hợp Slack: tạo VPC, Security Group, EC2 Instance, cấu hình Slack Incoming Webhooks <br> - Tạo Lambda Role, viết script Lambda bật/tắt EC2 tự động | 22/05/2026 | 22/05/2026 | https://www.youtube.com/playlist?list=PLahN4TLWtox2a3vElknwzU_urND8hLn1i |
| 6 | - Thực hành quản lý tag tài nguyên: lọc tài nguyên bằng tag qua Console và CLI, quản lý Resource Groups | 23/05/2026 | 23/05/2026 | https://www.youtube.com/playlist?list=PLahN4TLWtox2a3vElknwzU_urND8hLn1i |
| 7 | - Thực hành IAM nâng cao: tạo IAM User, Policy, Role, thực hành Switch Role và cấu hình chính sách phân quyền dựa trên tag (Tag-based access control) cho EC2 Tokyo / North Virginia | 24/05/2026 | 24/05/2026 | https://www.youtube.com/playlist?list=PLahN4TLWtox2a3vElknwzU_urND8hLn1i |

### Kết quả đạt được tuần 3:

* Thực hành AWS Storage Gateway: thiết lập Storage Gateway, tạo File Share và mount thành công lên thiết bị local.

* Thực hành Amazon FSx: tạo hệ thống tệp SSD/HDD Multi-AZ, cấu hình quotas, shadow copies, data deduplication và kiểm tra co giãn dung lượng/băng thông.

* Thực hành Amazon S3 & CloudFront: lưu trữ web tĩnh, quản lý chặn truy cập công khai, tích hợp CloudFront, quản lý phiên bản S3, di chuyển object và cấu hình Replication đa vùng.

* Theo dõi các video về Định danh & Bảo mật: Mô hình trách nhiệm chung, IAM, Cognito, AWS Organizations, Identity Center, KMS và tiêu chuẩn bảo mật Security Hub.

* Thực hành tích hợp Slack: viết hàm Lambda bật/tắt EC2 tự động và nhận thông báo trạng thái qua Slack Incoming Webhooks.

* Thực hành gắn tag tài nguyên: lọc tài nguyên qua Console và CLI, tạo Resource Group, cấu hình Switch Role và kiểm tra phân quyền IAM dựa trên tag.

* Đã hoàn thành theo dõi và học các video từ 121 - 186 trong series First Cloud Journey Bootcamp 2025 của AWS Study Group.

