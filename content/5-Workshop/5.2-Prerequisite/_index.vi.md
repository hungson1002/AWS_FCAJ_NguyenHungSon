---
title : "Chuẩn bị môi trường & Cognito"
date : 2024-01-01 
weight : 2
chapter : false
pre : " <b> 5.2. </b> "
---

### Mục tiêu

Cài đặt các công cụ CLI cần thiết, cấu hình thông tin xác thực AWS CLI và khởi tạo Amazon Cognito User Pool trực tiếp trên AWS Console.

---

#### Yêu cầu công cụ

| Công cụ | Phiên bản | Mục đích |
|---|---|---|
| **Node.js** | v20+ | Runtime cho Lambda và Frontend |
| **AWS CLI** | v2 | Tương tác với tài nguyên AWS từ terminal |
| **AWS SAM CLI** | latest | Build & deploy Serverless Backend stack |
| **Git** | any | Quản lý mã nguồn |

---

#### Bước 1: Cấu hình AWS CLI

```bash
aws configure
```

Nhập thông tin xác thực khi được hỏi:

```
AWS Access Key ID:     <access-key>
AWS Secret Access Key: <secret-key>
Default region:        ap-southeast-1
Output format:         json
```

Kiểm tra kết nối thành công:

```bash
aws sts get-caller-identity
```

---

#### Bước 2: Clone mã nguồn dự án

```bash
git clone https://github.com/HaoAboutMe/AWS_Cloud_Battleship_Arena.git
cd AWS_Cloud_Battleship_Arena
```

---

#### Bước 3: Khởi tạo Amazon Cognito User Pool (AWS Console)

1. Mở **AWS Console → Cognito → User pools → Create user pool**.
2. **Configure sign-in experience**: Chọn **Email**. Nhấn **Next**.
3. **Configure security requirements**: Giữ nguyên mặc định. Nhấn **Next**.
4. **Configure sign-up experience**: Giữ nguyên mặc định. Nhấn **Next**.
5. **Configure message delivery**: Chọn **Send email with Cognito**. Nhấn **Next**.
6. **Integrate app**:
   - **User Pool Name**: `Battleship-Arena`.
   - **Initial app client**: Chọn **Public client**.
   - **App client name**: `CloudBattleshipArena`.
   - Bỏ tích **Generate client secret**.
7. Nhấn **Next**, kiểm tra lại toàn bộ và nhấn **Create user pool**.

<img src="/images/5-Workshop/5.2-Prerequisite/cognito-user-pool.png" width="100%">

Sau khi tạo xong, lưu lại hai giá trị trong trang chi tiết User Pool:
- **User Pool ID** (ví dụ: `ap-southeast-1_VV7CeCaWL`)
- **Client ID** (ví dụ: `1vmsoep8dq5nlr1qvf8hpgsvs2`)

<img src="/images/5-Workshop/5.2-Prerequisite/cognito-app-client.png" width="100%">