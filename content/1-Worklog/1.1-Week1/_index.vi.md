---

title: "Worklog Tuần 1"
date: 2026-05-10
weight: 1
chapter: false
pre: " <b> 1.1. </b> "
----------------------

### Mục tiêu tuần 1:

* Làm quen với chương trình First Cloud AI Journey và môi trường AWS.
* Hiểu các dịch vụ AWS cơ bản và cách quản lý tài nguyên trên AWS.
* Thực hành các bài lab về EC2, VPC, VPC Peering và Transit Gateway.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc                                                                                                                                             | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                                                                      |
| --- | ----------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------------------------------------------------- |
| 2   | - Làm quen với chương trình First Cloud AI Journey <br> - Tìm hiểu tổng quan về Cloud Computing và AWS                                                | 04/05/2026   | 04/05/2026      | https://cloudjourney.awsstudygroup.com/                                             |
| 3   | - Tìm hiểu AWS Management Console <br> - Tìm hiểu Amazon EC2 cơ bản: <br>  + Instance Type <br>  + AMI <br>  + EBS <br>  + Security Group             | 05/05/2026   | 05/05/2026      | https://www.youtube.com/watch?v=AWXCQAd4_qM&list=PLahN4TLWtox2a3vElknwzU_urND8hLn1i |
| 4   | - Thực hành tạo EC2 Instance <br> - Tìm hiểu Elastic IP <br> - Thực hành SSH vào EC2 bằng PuTTY và MobaXterm                                          | 06/05/2026   | 06/05/2026      | https://cloudjourney.awsstudygroup.com/                                             |
| 5   | - Tìm hiểu Amazon VPC: <br>  + Subnet <br>  + Route Table <br>  + Internet Gateway <br>  + NAT Gateway                                                | 07/05/2026   | 08/05/2026      | https://cloudjourney.awsstudygroup.com/                                             |
| 6   | - Thực hành VPC Peering <br> - Cấu hình Route Table giữa các VPC <br> - Tìm hiểu Security Group trong môi trường nhiều VPC                            | 09/05/2026   | 09/05/2026      | https://cloudjourney.awsstudygroup.com/                                             |
| 7   | - Tìm hiểu AWS Transit Gateway <br> - Thực hành tạo Transit Gateway Attachment và Route Table <br> - Thực hành triển khai hạ tầng bằng CloudFormation | 10/05/2026   | 11/05/2026      | https://www.youtube.com/watch?v=AWXCQAd4_qM&list=PLahN4TLWtox2a3vElknwzU_urND8hLn1i |

### Kết quả đạt được tuần 1:

* Hiểu được mô hình cơ bản của Cloud Computing và các nhóm dịch vụ chính của AWS:

  * Compute
  * Storage
  * Networking
  * Security
  * ...

* Làm quen với AWS Management Console và cách quản lý tài nguyên AWS.

* Thực hành thành công:

  * Tạo EC2 Instance
  * Cấu hình Security Group
  * Gán Elastic IP
  * SSH vào EC2 bằng PuTTY/MobaXterm

* Hiểu được mô hình hoạt động của Amazon VPC:

  * Subnet
  * Route Table
  * Internet Gateway
  * NAT Gateway

* Thực hành VPC Peering và hiểu cách kết nối giữa nhiều VPC.

* Tìm hiểu AWS Transit Gateway:

  * Transit Gateway Attachment
  * Transit Gateway Route Table
  * Routing giữa các VPC

* Làm quen với AWS CloudFormation và triển khai tài nguyên bằng template có sẵn.

* Thực hành xử lý một số lỗi thường gặp:

  * SSH connection
  * Security Group
  * Route configuration
  * CloudFormation rollback
  * EC2 Instance Type
  * ...

* Đã hoàn thành theo dõi và học các video từ 1 - 71 trong series First Cloud Journey Bootcamp 2025 của AWS Study Group.

---

## Nguyên Hùng Sơn

### Mục tiêu tuần 1:

* Làm quen với chương trình First Cloud AI Journey và môi trường AWS.
* Hiểu các khái niệm cơ bản về Cloud Computing và các dịch vụ AWS.
* Thiết lập AWS Account, cấu hình MFA, IAM Admin User, Budget Alert và Support Plan.
* Tìm hiểu và thực hành mạng Amazon VPC: Subnet, Route Table, Internet Gateway, NAT Gateway, Security Group, NACL.
* Thực hành Hybrid DNS với Route 53 Resolver và cấu hình VPC Peering.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc                                                                                                                                                                                                          | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                                                            |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------ | --------------- | ------------------------------------------------------------------------- |
| 2   | - Làm quen với chương trình First Cloud AI Journey <br> - Tìm hiểu tổng quan về Cloud Computing <br> - Tìm hiểu AWS Global Infrastructure và Management Console                                                   | 04/05/2026   | 04/05/2026      | https://www.youtube.com/playlist?list=PLahN4TLWtox2a3vElknwzU_urND8hLn1i |
| 3   | - Tìm hiểu Cost Optimization và Support Plan trên AWS <br> - Thực hành thiết lập AWS Account: <br>  + Bật MFA <br>  + Tạo IAM Admin User <br>  + Cấu hình Budget Alert <br>  + Kích hoạt AWS Support Plan         | 05/05/2026   | 05/05/2026      | https://www.youtube.com/playlist?list=PLahN4TLWtox2a3vElknwzU_urND8hLn1i |
| 4   | - Tìm hiểu Amazon VPC: <br>  + Kiến trúc VPC <br>  + Subnet (Public/Private) <br>  + Route Table <br>  + Internet Gateway <br>  + NAT Gateway <br> - Tìm hiểu bảo mật mạng (Security Group, NACL)                | 06/05/2026   | 06/05/2026      | https://www.youtube.com/playlist?list=PLahN4TLWtox2a3vElknwzU_urND8hLn1i |
| 5   | - Thực hành xây dựng môi trường VPC: <br>  + Tạo VPC và Subnet <br>  + Cấu hình Route Table và Internet Gateway <br>  + Triển khai NAT Gateway <br>  + Cấu hình Security Group và NACL <br>  + Tạo EC2 Instance   | 07/05/2026   | 08/05/2026      | https://www.youtube.com/playlist?list=PLahN4TLWtox2a3vElknwzU_urND8hLn1i |
| 6   | - Tìm hiểu Hybrid DNS với Amazon Route 53 Resolver <br>  + Inbound Endpoint <br>  + Outbound Endpoint <br>  + Forwarding Rule <br> - Thực hành phân giải DNS giữa VPC và môi trường On-premises                    | 09/05/2026   | 09/05/2026      | https://www.youtube.com/playlist?list=PLahN4TLWtox2a3vElknwzU_urND8hLn1i |
| 7   | - Tìm hiểu VPC Peering <br> - Thực hành tạo kết nối VPC Peering <br> - Cấu hình Route Table cho phép giao tiếp giữa các VPC <br> - Kiểm tra kết nối giữa EC2 Instance ở các VPC khác nhau                          | 10/05/2026   | 11/05/2026      | https://www.youtube.com/playlist?list=PLahN4TLWtox2a3vElknwzU_urND8hLn1i |

### Kết quả đạt được tuần 1:

* Theo dõi và tìm hiểu được mô hình cơ bản của Cloud Computing và các nhóm dịch vụ chính của AWS (Compute, Storage, Networking, Security, ...).

* Làm quen với AWS Management Console và AWS Global Infrastructure (Region, Availability Zone, Edge Location).

* Thiết lập thành công AWS Account: bật MFA, tạo IAM Admin User, cấu hình Budget Alert và kích hoạt AWS Support Plan.

* Tìm hiểu các nguyên tắc tối ưu chi phí và các gói hỗ trợ trên AWS.

* Theo dõi các video về Amazon VPC gồm: VPC, CIDR Block, Public/Private Subnet, Route Table, Internet Gateway, NAT Gateway, Security Group và NACL.

* Thực hành xây dựng môi trường VPC hoàn chỉnh: tạo VPC, Subnet, cấu hình Route Table, IGW, NAT Gateway, Security Group/NACL và khởi tạo EC2 Instance.

* Tìm hiểu Hybrid DNS với Route 53 Resolver: Inbound/Outbound Endpoint, Forwarding Rule, phân giải DNS giữa VPC và On-premises.

* Thực hành VPC Peering: tạo kết nối, cấu hình Route Table và kiểm tra kết nối xuyên VPC.

* Thực hành xử lý các lỗi thường gặp: Route Table, Security Group, NACL, NAT Gateway, DNS resolution, ...

* Đã hoàn thành theo dõi và học các video từ 1 - 64 trong series First Cloud Journey Bootcamp 2025 của AWS Study Group.
