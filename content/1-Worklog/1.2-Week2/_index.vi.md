---

title: "Worklog Tuần 2"
date: 2026-05-17
weight: 2
chapter: false
pre: " <b> 1.2. </b> "
----------------------

### Mục tiêu tuần 2:

* Tìm hiểu dịch vụ Amazon S3 và mô hình Object Storage.
* Thực hành triển khai Static Website Hosting bằng Amazon S3 và CloudFront.
* Hiểu cơ chế phân quyền, versioning và replication trong Amazon S3.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc                                                                                                                                    | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                                                                      |
| --- | -------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------------------------------------------------- |
| 2   | - Tìm hiểu tổng quan Amazon S3 <br>  + Bucket <br>  + Object <br>  + Storage Class <br>  + Bucket Policy                                     | 11/05/2026   | 11/05/2026      | https://www.youtube.com/playlist?list=PLahN4TLWtox2a3vElknwzU_urND8hLn1i/                                             |
| 3   | - Thực hành tạo và quản lý S3 Bucket <br> - Upload và quản lý object trên Amazon S3 <br> - Tìm hiểu Static Website Hosting                   | 12/05/2026   | 12/05/2026      | https://www.youtube.com/playlist?list=PLahN4TLWtox2a3vElknwzU_urND8hLn1i/                                               |
| 4   | - Cấu hình Bucket Policy và Public Access <br> - Thực hành kiểm tra truy cập website public <br> - Tìm hiểu quyền truy cập object trong S3   | 13/05/2026   | 14/05/2026      | https://www.youtube.com/playlist?list=PLahN4TLWtox2a3vElknwzU_urND8hLn1i/                                               |
| 5   | - Tìm hiểu Amazon CloudFront <br> - Cấu hình CloudFront Distribution cho S3 Static Website <br> - Kiểm tra phân phối nội dung qua CloudFront | 15/05/2026   | 15/05/2026      | https://www.youtube.com/playlist?list=PLahN4TLWtox2a3vElknwzU_urND8hLn1i/                                               |
| 6   | - Tìm hiểu S3 Bucket Versioning <br> - Thực hành quản lý version của object <br> - Tìm hiểu lifecycle và di chuyển object                    | 16/05/2026   | 16/05/2026      | https://www.youtube.com/playlist?list=PLahN4TLWtox2a3vElknwzU_urND8hLn1i/                                               |
| 7   | - Tìm hiểu S3 Cross-Region Replication <br> - Thực hành replication object đa Region <br> - Tìm hiểu tính sẵn sàng và backup trên Amazon S3  | 17/05/2026   | 17/05/2026      | https://www.youtube.com/playlist?list=PLahN4TLWtox2a3vElknwzU_urND8hLn1i/                                               |

### Kết quả đạt được tuần 2:

* Hiểu được kiến trúc và mô hình lưu trữ Object Storage của Amazon S3:

  * Bucket
  * Object
  * Storage Class
  * Bucket Policy
  * Access Control
  * ...

* Thực hành thành công:

  * Tạo và quản lý S3 Bucket
  * Upload và quản lý object
  * Triển khai Static Website Hosting
  * Kiểm tra truy cập website

* Hiểu và cấu hình được Bucket Policy và Public Access cho website tĩnh trên Amazon S3.

* Tìm hiểu cơ bản về Amazon CloudFront và mô hình CDN:

  * CloudFront Distribution
  * Origin Configuration
  * Edge Location
  * Content Delivery

* Thực hành cấu hình CloudFront cho website tĩnh sử dụng Amazon S3 làm Origin.

* Hiểu được cơ chế hoạt động của S3 Bucket Versioning và quản lý nhiều phiên bản object.

* Tìm hiểu về cơ chế replication và backup trên Amazon S3:

  * Cross-Region Replication (CRR)
  * Multi-region availability
  * Data redundancy

* Thực hành xử lý các lỗi thường gặp:

  * Bucket Policy syntax
  * Access Denied
  * Public Access Block
  * CloudFront configuration
  * Object permission
  * ...

* Đã hoàn thành theo dõi và học các video từ 72 - 102 trong series First Cloud Journey Bootcamp 2025 của AWS Study Group.

---

## Nguyên Hùng Sơn

### Mục tiêu tuần 2:

* Hoàn thành các bài lab AWS Transit Gateway để kết thúc Module 02 về mạng.
* Tìm hiểu sâu về Amazon EC2: Instance Type, AMI, EBS, Instance Store, User Data, Metadata và Auto Scaling.
* Thực hành AWS Backup cho EC2 và hiểu quy trình backup/restore.
* Tìm hiểu và thực hành AWS Storage Gateway và cấu hình File Share.
* Tìm hiểu Amazon S3: Static Website Hosting, tích hợp CloudFront, Bucket Versioning và Cross-Region Replication.
* Hiểu tổng quan dịch vụ Storage AWS và bắt đầu migrate Virtual Machine từ On-premises lên AWS.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc                                                                                                                                                                                                               | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                                                            |
| --- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ------------------------------------------------------------------------- |
| 2   | - Hoàn thành lab Transit Gateway: <br>  + Tạo Transit Gateway <br>  + Tạo Attachment và Route Table <br>  + Cập nhật Route Table VPC <br> - Tìm hiểu EC2: Instance Type, AMI, Key Pair                                 | 11/05/2026   | 11/05/2026      | https://www.youtube.com/playlist?list=PLahN4TLWtox2a3vElknwzU_urND8hLn1i |
| 3   | - Tìm hiểu EC2 nâng cao: <br>  + Elastic Block Store (EBS) <br>  + Instance Store <br>  + User Data (bootstrap script) <br>  + EC2 Metadata Service <br>  + EC2 Auto Scaling <br>  + EFS / FSx / Lightsail / MGN      | 12/05/2026   | 12/05/2026      | https://www.youtube.com/playlist?list=PLahN4TLWtox2a3vElknwzU_urND8hLn1i |
| 4   | - Thực hành AWS Backup: <br>  + Triển khai hạ tầng Backup <br>  + Tạo Backup Plan <br>  + Kiểm tra Restore từ Recovery Point <br> - Thực hành Storage Gateway: <br>  + Tạo S3 Bucket <br>  + Tạo EC2 cho Storage Gateway | 13/05/2026   | 14/05/2026      | https://www.youtube.com/playlist?list=PLahN4TLWtox2a3vElknwzU_urND8hLn1i |
| 5   | - Thực hành Storage Gateway: <br>  + Tạo Storage Gateway <br>  + Cấu hình File Share <br> - Tìm hiểu và thực hành S3 Static Website Hosting: <br>  + Tạo S3 Bucket <br>  + Cấu hình Public Access và Bucket Policy    | 15/05/2026   | 15/05/2026      | https://www.youtube.com/playlist?list=PLahN4TLWtox2a3vElknwzU_urND8hLn1i |
| 6   | - Cấu hình CloudFront cho S3 Static Website <br> - Kiểm tra phân phối nội dung qua CloudFront <br> - Tìm hiểu S3 Bucket Versioning <br> - Thực hành lifecycle object và Cross-Region Replication                       | 16/05/2026   | 16/05/2026      | https://www.youtube.com/playlist?list=PLahN4TLWtox2a3vElknwzU_urND8hLn1i |
| 7   | - Tìm hiểu lý thuyết Module 04 Storage: S3 nâng cao, Snow Family, Storage Gateway, Backup <br> - Thực hành AWS Backup với thông báo <br> - Bắt đầu migrate VM: VMware Workstation, Export VM, Upload và Import lên AWS  | 17/05/2026   | 17/05/2026      | https://www.youtube.com/playlist?list=PLahN4TLWtox2a3vElknwzU_urND8hLn1i |

### Kết quả đạt được tuần 2:

* Hoàn thành thiết lập AWS Transit Gateway: tạo Gateway, Attachment cho nhiều VPC, Route Table và cập nhật định tuyến liên VPC.

* Theo dõi và tìm hiểu được các khái niệm về Amazon EC2: Instance Type, AMI, Key Pair, EBS, Instance Store, User Data, Metadata Service và Auto Scaling.

* Tìm hiểu thêm các dịch vụ liên quan: EFS, FSx, Amazon Lightsail và AWS MGN.

* Thực hành AWS Backup: triển khai hạ tầng Backup, tạo Backup Plan, Recovery Point và restore EC2 Instance thành công.

* Tìm hiểu và thực hành AWS Storage Gateway: tạo Gateway và cấu hình File Share với Amazon S3.

* Theo dõi các video về Amazon S3: Bucket, Object, Storage Class, Access Point, Bucket Policy và kiểm soát Public Access.

* Thực hành S3 Static Website Hosting và CloudFront: cấu hình Bucket Policy, bật Static Website, thiết lập CloudFront Distribution và kiểm tra phân phối nội dung.

* Tìm hiểu tính năng quản lý dữ liệu S3: Bucket Versioning, Lifecycle và Cross-Region Replication (CRR).

* Theo dõi tổng quan dịch vụ Storage AWS: S3, Glacier, Snow Family, Storage Gateway và AWS Backup.

* Bắt đầu migrate Virtual Machine từ On-premises lên AWS: export VM từ VMware Workstation, upload lên S3, import và triển khai EC2 từ AMI đã import.

* Thực hành xử lý các lỗi thường gặp: Transit Gateway Attachment, EBS, Bucket Policy, CloudFront origin, S3 Replication, import VM, ...

* Đã hoàn thành theo dõi và học các video từ 65 - 120 trong series First Cloud Journey Bootcamp 2025 của AWS Study Group.
