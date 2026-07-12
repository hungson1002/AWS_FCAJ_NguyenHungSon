---
title : "Cài đặt Amazon Cognito"
date : 2026-07-10  
weight : 2
chapter : false
pre : " <b> 5.2.2. </b> "
---

### Mục tiêu

Khởi tạo Amazon Cognito User Pool trực tiếp trên AWS Console để phục vụ xác thực người dùng cho ứng dụng Cloud Battleship Arena.

---

#### Bước 1: Tạo Cognito User Pool

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

---

#### Bước 2: Lưu thông tin User Pool

Sau khi tạo xong, vào trang chi tiết User Pool và lưu lại hai giá trị:
- **User Pool ID** (dạng `<YOUR_COGNITO_USER_POOL_ID>`)
- **Client ID** (dạng `<YOUR_COGNITO_USER_POOL_CLIENT_ID>`)

<img src="/images/5-Workshop/5.2-Prerequisite/cognito-app-client.png" width="100%">

> 📌 Hai giá trị này sẽ được dùng để cấu hình biến môi trường cho Frontend ở bước 5.5.
