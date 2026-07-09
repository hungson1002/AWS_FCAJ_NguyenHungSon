---
title : "Dọn dẹp tài nguyên"
date : 2024-01-01
weight : 6
chapter : false
pre : " <b> 5.6. </b> "
---

Chúc mừng bạn đã hoàn thành workshop **Cloud Battleship Arena Serverless Backend**!

Trong workshop này, bạn đã học và thực hành:
+ **AWS SAM** — Định nghĩa và triển khai hạ tầng Serverless dưới dạng mã (IaC).
+ **AWS Lambda** — Xây dựng logic game không máy chủ với Node.js 24.x.
+ **API Gateway HTTP API** — Cung cấp RESTful endpoints cho quản lý phòng và hồ sơ người dùng với Cognito JWT Authorizer.
+ **API Gateway WebSocket API** — Tạo kênh giao tiếp hai chiều real-time cho gameplay.
+ **Amazon DynamoDB** — Lưu trữ trạng thái game với On-demand billing và TTL tự động.
+ **Amazon Cognito** — Bảo vệ API với JWT token và nguyên tắc Least Privilege.

#### Dọn dẹp

Để tránh phát sinh chi phí không cần thiết, hãy xóa tất cả tài nguyên đã tạo trong workshop.

**Bước 1: Xóa SAM Backend Stack**

```bash
cd BackEnd
sam delete --stack-name cloud-battleship-backend
```

SAM sẽ xác nhận trước khi xóa. Nhập `y` để xác nhận. Quá trình xóa mất khoảng 2–3 phút.

**Bước 2: Xóa S3 Bucket chứa artifact SAM**

SAM tạo một S3 bucket để lưu deployment artifacts. Xóa thủ công:

```bash
# Lấy tên bucket (thường có prefix "aws-sam-cli-managed-default")
aws s3 ls | grep aws-sam-cli

# Xóa tất cả object trong bucket
aws s3 rm s3://<bucket-name> --recursive

# Xóa bucket
aws s3 rb s3://<bucket-name>
```

**Bước 3: Xóa Cognito User Pool**

```bash
# Xóa App Client trước
aws cognito-idp delete-user-pool-client \
  --user-pool-id <USER_POOL_ID> \
  --client-id <CLIENT_ID>

# Xóa User Pool
aws cognito-idp delete-user-pool \
  --user-pool-id <USER_POOL_ID>
```

**Bước 4: Kiểm tra CloudFormation Console**

Mở **AWS Console → CloudFormation → Stacks** và xác nhận stack `cloud-battleship-backend` đã được xóa hoàn toàn (trạng thái `DELETE_COMPLETE`).

**Bước 5: Kiểm tra DynamoDB Console**

Mở **AWS Console → DynamoDB → Tables** và xác nhận các bảng sau đã bị xóa:
+ `Rooms`
+ `Connections`
+ `ChatMessages`

**Bước 6: Kiểm tra Lambda Console**

Mở **AWS Console → Lambda → Functions** và xác nhận không còn function nào từ workshop này.

#### Lưu ý

> ⚠️ Nếu bạn đã deploy Frontend stack riêng (`frontend-hosting.yaml`), hãy xóa stack đó trong CloudFormation Console hoặc chạy `aws cloudformation delete-stack --stack-name <frontend-stack-name>`.