---
title: "Giới thiệu"
date: 2024-01-01 
weight: 1
chapter: false
pre: " <b> 5.1. </b> "
---

### Mục tiêu

Sau khi hoàn thành workshop này, bạn sẽ nắm vững quy trình từ đầu đến cuối để triển khai một ứng dụng Serverless thời gian thực đa người chơi lên AWS — từ Backend API, Frontend CDN, CI/CD tự động hóa đến bảo mật IAM/Cognito.

---

#### Các dịch vụ AWS sử dụng trong Workshop

| Dịch vụ | Vai trò trong dự án |
| :--- | :--- |
| **AWS SAM CLI** | Định nghĩa toàn bộ tài nguyên Backend (IaC), đóng gói và tự động deploy. |
| **AWS Lambda** | Xử lý logic game (tạo phòng, ghép trận, xác định phát bắn, cập nhật rank). |
| **Amazon API Gateway** | HTTP API tiếp nhận REST requests. WebSocket API duy trì kênh thời gian thực. |
| **Amazon DynamoDB** | Lưu trữ phòng chơi, kết nối WebSocket, lịch sử đấu và điểm rank người dùng. |
| **Amazon Cognito** | Quản lý đăng ký, đăng nhập và cung cấp JWT Token bảo mật API. |
| **Amazon S3** | Lưu trữ mã nguồn tĩnh (HTML, CSS, JS) của giao diện Frontend React 19. |
| **Amazon CloudFront** | CDN phân phối nội dung toàn cầu, tăng tốc độ tải trang và bảo vệ S3 qua OAC. |
| **GitHub Actions + OIDC** | Tự động hóa quy trình build và deploy Frontend khi có push code lên nhánh `main`. |

---

#### Lộ trình thực hành

1. **Chuẩn bị**: Cấu hình AWS CLI và khởi tạo Cognito User Pool.
2. **Backend**: Build & Deploy Backend Serverless bằng AWS SAM. Kiểm thử HTTP API.
3. **Real-time**: Kết nối WebSocket, gửi nhận gói tin và kiểm tra dữ liệu DynamoDB.
4. **Frontend**: Deploy S3 Bucket + CloudFront Distribution qua CloudFormation.
5. **CI/CD**: Cấu hình IAM OIDC và GitHub Actions Workflow tự động hóa hoàn toàn.
6. **Bảo mật & Dọn dẹp**: Rà soát phân quyền Least Privilege và xóa tài nguyên.
