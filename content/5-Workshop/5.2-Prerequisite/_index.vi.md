---
title : "Chuẩn bị môi trường & Cognito"
date : 2026-07-10 
weight : 2
chapter : false
pre : " <b> 5.2. </b> "
---

### Mục tiêu

Thiết lập toàn bộ nền tảng cần thiết trước khi triển khai hệ thống — bao gồm tạo IAM User, cấu hình AWS CLI với Access Key và khởi tạo **Amazon Cognito User Pool** phục vụ xác thực người dùng cho game **Cloud Battleship Arena**.

---

#### Tại sao cần chuẩn bị trước bước này?

1. **Quyền truy cập AWS**: IAM User và Access Key là điều kiện tiên quyết để AWS CLI và SAM CLI có thể tương tác với tài khoản AWS của bạn.
2. **Xác thực người dùng**: Cognito User Pool phải được tạo sẵn vì **User Pool ID** và **Client ID** sẽ được truyền vào làm tham số khi deploy SAM Backend ở bước 5.3.
3. **Mã nguồn dự án**: Clone repository về máy trước để sẵn sàng cho các bước build và deploy tiếp theo.

---

#### Nội dung bài học

- [Chuẩn bị môi trường](5.2.1-Environment-Setup/)
- [Cài đặt Amazon Cognito](5.2.2-Cognito-Setup/)