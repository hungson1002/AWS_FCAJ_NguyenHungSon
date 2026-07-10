---
title : "Chuẩn bị môi trường"
date : 2024-01-01 
weight : 1
chapter : false
pre : " <b> 5.2.1. </b> "
---

### Mục tiêu

Tạo IAM User với quyền hạn phù hợp, cấp phát Access Key để xác thực AWS CLI, cài đặt các công cụ cần thiết và clone mã nguồn dự án.

---

#### Yêu cầu công cụ

| Công cụ | Phiên bản | Mục đích |
|---|---|---|
| **Node.js** | v20+ | Runtime cho Lambda và Frontend |
| **AWS CLI** | v2 | Tương tác với tài nguyên AWS từ terminal |
| **AWS SAM CLI** | latest | Build & deploy Serverless Backend stack |
| **Git** | any | Quản lý mã nguồn |

---

#### Bước 1: Tạo IAM User

1. Truy cập **AWS Console** → tìm và mở dịch vụ **IAM**.

<img src="/images/5-Workshop/5.2-Prerequisite/5.2.1-Environment-Setup/access-iam.jpg" width="100%">

2. Ở menu bên trái, chọn **Users** → nhấn **Create user**.

<img src="/images/5-Workshop/5.2-Prerequisite/5.2.1-Environment-Setup/createUser.jpg" width="100%">

3. Nhập **User name** (ví dụ: `battleship-deployer`). Tích vào ô **Provide user access to the AWS Management Console** để cấp quyền đăng nhập Console và đặt mật khẩu. Nhấn **Next**.

<img src="/images/5-Workshop/5.2-Prerequisite/5.2.1-Environment-Setup/setup-username-password.jpg" width="100%">

4. Ở bước **Set permissions**, chọn **Attach policies directly**. Tìm và tích chọn **AdministratorAccess**. Nhấn **Next**.

<img src="/images/5-Workshop/5.2-Prerequisite/5.2.1-Environment-Setup/setup-policy.jpg" width="100%">

5. Kiểm tra lại toàn bộ thông tin ở bước **Review and create**. Nhấn **Create user**.

<img src="/images/5-Workshop/5.2-Prerequisite/5.2.1-Environment-Setup/review.jpg" width="100%">

6. Tạo user thành công. Nhấn **Download .csv file** để lưu lại thông tin đăng nhập Console (username, password, URL).

> ⚠️ File CSV chỉ có thể tải **một lần duy nhất** tại thời điểm này. Hãy lưu vào nơi an toàn trước khi đóng trang.

<img src="/images/5-Workshop/5.2-Prerequisite/5.2.1-Environment-Setup/success.jpg" width="100%">

---

#### Bước 2: Tạo Access Key

1. Vào trang chi tiết của user vừa tạo → tab **Security credentials**.
2. Cuộn xuống phần **Access keys** → nhấn **Create access key**.

<img src="/images/5-Workshop/5.2-Prerequisite/5.2.1-Environment-Setup/createAccessKey.jpg" width="100%">

3. Chọn use case **Command Line Interface (CLI)** → xác nhận → nhấn **Next**.
4. Nhập mô tả tag (tùy chọn) → nhấn **Create access key**.

<img src="/images/5-Workshop/5.2-Prerequisite/5.2.1-Environment-Setup/SetDescriptionTag.jpg" width="100%">

5. **Lưu lại ngay** hai giá trị:
   - **Access Key ID**
   - **Secret Access Key**

> ⚠️ Secret Access Key chỉ hiển thị **một lần duy nhất**. Hãy lưu vào nơi an toàn trước khi đóng trang.

<img src="/images/5-Workshop/5.2-Prerequisite/5.2.1-Environment-Setup/ReviewAccessKey.jpg" width="100%">

---

#### Bước 3: Cấu hình AWS CLI

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

#### Bước 4: Clone mã nguồn dự án

```bash
git clone https://github.com/HaoAboutMe/AWS_Cloud_Battleship_Arena.git
cd AWS_Cloud_Battleship_Arena
```
