---
title : "Chuẩn bị môi trường"
date : 2024-01-01 
weight : 2
chapter : false
pre : " <b> 5.2. </b> "
---

#### Yêu cầu hệ thống

Trước khi bắt đầu workshop, hãy đảm bảo bạn đã cài đặt và cấu hình các công cụ sau:

| Công cụ | Phiên bản | Mục đích |
|---|---|---|
| **Node.js** | v20+ | Môi trường chạy Lambda và Frontend |
| **AWS CLI** | v2 | Tương tác với AWS từ terminal |
| **AWS SAM CLI** | latest | Build và deploy serverless stack |
| **Git** | any | Quản lý mã nguồn |

#### Bước 1: Cài đặt AWS CLI

Tải và cài đặt AWS CLI v2 từ trang chính thức của AWS. Sau khi cài đặt, cấu hình credentials:

```bash
aws configure
```

Nhập các thông tin sau khi được yêu cầu:
```
AWS Access Key ID [None]: <access-key-của-bạn>
AWS Secret Access Key [None]: <secret-key-của-bạn>
Default region name [None]: ap-southeast-1
Default output format [None]: json
```

#### Bước 2: Cài đặt AWS SAM CLI

```bash
# Windows (dùng MSI installer từ trang AWS SAM)
# Hoặc dùng pip:
pip install aws-sam-cli
```

Kiểm tra cài đặt:
```bash
sam --version
```

#### Bước 3: Cài đặt IAM permissions

Gắn IAM permission policy sau vào IAM user/role của bạn để có đủ quyền triển khai và dọn dẹp tài nguyên trong workshop này:

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "BattleshipWorkshopPermissions",
            "Effect": "Allow",
            "Action": [
                "cloudformation:*",
                "cloudwatch:*",
                "cognito-idp:*",
                "dynamodb:*",
                "apigateway:*",
                "lambda:*",
                "s3:*",
                "iam:CreateRole",
                "iam:DeleteRole",
                "iam:AttachRolePolicy",
                "iam:DetachRolePolicy",
                "iam:PutRolePolicy",
                "iam:DeleteRolePolicy",
                "iam:GetRole",
                "iam:GetRolePolicy",
                "iam:PassRole",
                "iam:ListRoles",
                "iam:CreateInstanceProfile",
                "iam:DeleteInstanceProfile",
                "iam:AddRoleToInstanceProfile",
                "iam:RemoveRoleFromInstanceProfile",
                "logs:*",
                "sns:*",
                "ssm:GetParameters"
            ],
            "Resource": "*"
        }
    ]
}
```

#### Bước 4: Clone mã nguồn dự án

```bash
git clone https://github.com/hungson1002/AWS_Cloud_Battleship_Arena.git
cd AWS_Cloud_Battleship_Arena
```

#### Bước 5: Cài đặt Cognito User Pool (Điều kiện tiên quyết)

Backend yêu cầu một Cognito User Pool đã được tạo sẵn. Tạo User Pool qua AWS Console hoặc CLI:

```bash
# Tạo User Pool
aws cognito-idp create-user-pool \
  --pool-name "CloudBattleshipArena" \
  --auto-verified-attributes email \
  --region ap-southeast-1

# Tạo App Client (ghi lại UserPoolId và ClientId)
aws cognito-idp create-user-pool-client \
  --user-pool-id <USER_POOL_ID> \
  --client-name "battleship-client" \
  --no-generate-secret \
  --region ap-southeast-1
```

Ghi lại hai giá trị quan trọng:
- `UserPoolId` — Ví dụ: `ap-southeast-1_XXXXXXXXX`
- `ClientId` — Ví dụ: `1abc2defgh3ijklmnop4qrstu`

Bạn sẽ cần chúng ở bước tiếp theo khi deploy Backend bằng SAM.