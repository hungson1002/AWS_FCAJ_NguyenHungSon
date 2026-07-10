---
title : "Triển khai Backend với AWS SAM"
date : 2024-01-01 
weight : 3
chapter : false
pre : " <b> 5.3. </b> "
---

### Mục tiêu

Triển khai thành công toàn bộ Backend của Cloud Battleship Arena lên AWS — bao gồm **8 Lambda functions**, **2 API Gateways** (HTTP + WebSocket) và **3 DynamoDB tables** — sử dụng mẫu cấu hình `template.yaml`.

---

#### Cấu trúc tài nguyên trong template.yaml

File `BackEnd/template.yaml` định nghĩa toàn bộ tài nguyên Backend bao gồm:
- **Globals**: Cấu hình chung cho cả 8 Lambda (Runtime `nodejs24.x`, Timeout 10 giây, và các biến môi trường dùng chung).
- **BattleshipHttpApi**: HTTP API Gateway với bộ xác thực JWT (JWT Authorizer) tích hợp với Amazon Cognito.
- **BattleshipWebSocketApi**: WebSocket API Gateway sử dụng route selection qua trường dữ liệu `action` trong tin nhắn gửi lên.
- **RoomsTable / ConnectionsTable / ChatMessagesTable**: 3 bảng DynamoDB lưu trữ tương ứng phòng đấu, kết nối socket hiện hoạt và lịch sử tin nhắn trò chuyện (hỗ trợ TTL tự động xóa và chỉ mục phụ GSI).

---

#### Bước 1: Cài đặt dependencies

```bash
cd AWS_Cloud_Battleship_Arena/BackEnd
npm install
```

---

#### Bước 2: Build SAM artifact

```bash
sam build
```

SAM sẽ phân tích tệp `template.yaml`, tự động chạy `npm install` đóng gói mã nguồn và dependencies của mỗi Lambda function vào thư mục cục bộ `.aws-sam/build/`.

---

#### Bước 3: Validate template (tùy chọn)

```bash
sam validate --lint
```

---

#### Bước 4: Deploy lên AWS

```bash
sam deploy --guided
```

Nhập các thông số cấu hình khi được hỏi:

| Thông số | Giá trị |
|---|---|
| Stack Name | `cloud-battleship-backend-dev` |
| AWS Region | `ap-southeast-1` |
| CognitoUserPoolId | `<YOUR_COGNITO_USER_POOL_ID>` |
| CognitoUserPoolClientId | `<YOUR_COGNITO_USER_POOL_CLIENT_ID>` |
| CorsOrigin | `http://localhost:5173` |

Sau khoảng 3–5 phút, quá trình triển khai hoàn tất. Lưu lại các giá trị URL từ phần **Outputs** của SAM:

```
HttpApiUrl    → https://elh9fh33rd.execute-api.ap-southeast-1.amazonaws.com
WebSocketUrl  → wss://b9mxr6sqg6.execute-api.ap-southeast-1.amazonaws.com/prod
```

{{% notice tip %}}
Sau lần đầu chạy `sam deploy --guided`, SAM tự lưu tham số vào `samconfig.toml`. Các lần deploy tiếp theo chỉ cần chạy lệnh `sam deploy` mà không cần thêm tham số `--guided`.
{{% /notice %}}

---

#### Bước 5: Cấu hình Cognito Post-Confirmation Trigger

Để hệ thống tự động lưu thông tin người chơi vào bảng DynamoDB sau khi họ xác nhận tài khoản (đăng ký thành công), chúng ta cần liên kết sự kiện đăng ký của Cognito với Lambda function `CreateUser`:

1. Truy cập **Amazon Cognito Console** > Chọn User Pool `Battleship-Arena` của bạn.
2. Chọn tab **User pool properties**.
3. Cuộn xuống phần **Lambda triggers** và chọn **Add Lambda trigger**.
4. Cấu hình các thông tin trigger:
   - **Trigger type**: Chọn **Sign-up** > Tích chọn **Post confirmation trigger** (Kích hoạt sau khi xác nhận đăng ký thành công).
   - **Assign Lambda function**: Chọn **Choose an existing Lambda function**.
   - **Lambda function**: Chọn hàm Lambda `CreateUser` (tên sẽ khớp dạng `cloud-battleship-backend-dev-CreateUserFunction-xxxxxx`).
5. Nhấp **Add Lambda trigger** để lưu cấu hình.

---

#### Kiểm tra kết quả (AWS Console)

**CloudFormation Stack:**
1. Mở **AWS Console → CloudFormation → Stacks**.
2. Xác nhận stack `cloud-battleship-backend-dev` đang ở trạng thái `CREATE_COMPLETE`.

![CloudFormation stack CREATE_COMPLETE](/images/5-Workshop/5.3-SAM-Backend/cloudformation-stack.png)

**DynamoDB Tables:**
1. Mở **AWS Console → DynamoDB → Tables**.
2. Xác nhận 3 bảng `Rooms`, `Connections`, và `User` đã được khởi tạo thành công.

![DynamoDB 3 tables](/images/5-Workshop/5.3-SAM-Backend/dynamodb-tables.png)

**API Gateway Routes:**
1. Mở **AWS Console → API Gateway → APIs**.
2. Chọn HTTP API của dự án (ví dụ: `cloud-battleship-backend-dev`).
3. Chọn **Routes** ở menu bên trái. Xác nhận tất cả các endpoint của HTTP API đều hiển thị đúng cấu trúc bên dưới route `/api`.

![Cấu trúc routes của API Gateway](/images/5-Workshop/5.3-SAM-Backend/AllAPI.jpg)

**Lambda Functions:**
1. Mở **AWS Console → Lambda → Functions**.
2. Xác nhận tất cả các Lambda functions của dự án đã được khởi tạo đầy đủ và sử dụng runtime `Node.js 24.x`.

![Danh sách các Lambda function](/images/5-Workshop/5.3-SAM-Backend/AllLambda.jpg)
