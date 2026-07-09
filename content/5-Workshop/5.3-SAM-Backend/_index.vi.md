---
title : "Triển khai Backend với AWS SAM"
date : 2024-01-01 
weight : 3
chapter : false
pre : " <b> 5.3. </b> "
---

#### Giới thiệu về AWS SAM

**AWS SAM (Serverless Application Model)** là framework IaC (Infrastructure as Code) giúp bạn định nghĩa toàn bộ hạ tầng Serverless bằng một file YAML duy nhất (`template.yaml`). SAM tự động tạo các tài nguyên AWS như Lambda, API Gateway, DynamoDB, IAM Roles, v.v.

#### Cấu trúc file template.yaml

File `BackEnd/template.yaml` của Cloud Battleship Arena định nghĩa:

**Tài nguyên DynamoDB:**
- `RoomsTable` — Lưu trạng thái phòng chơi, có TTL tự động xóa phòng hết hạn.
- `ConnectionsTable` — Ánh xạ `connectionId` ↔ `roomCode` cho WebSocket, có GSI `byRoomCode`.
- `ChatMessagesTable` — Lưu lịch sử tin nhắn chat trong phòng, có TTL.

**Tài nguyên API Gateway:**
- `BattleshipHttpApi` — HTTP API với CORS và Cognito JWT Authorizer.
- `BattleshipWebSocketApi` — WebSocket API với route selection theo `$request.body.action`.

**Tài nguyên Lambda (ví dụ):**
```yaml
CreateRoomFunction:
  Type: AWS::Serverless::Function
  Properties:
    FunctionName: CreateRoom
    Handler: src/handlers/createRoom.handler
    Runtime: nodejs24.x
    Policies:
      - DynamoDBCrudPolicy:
          TableName: !Ref RoomsTable
    Events:
      CreateRoom:
        Type: HttpApi
        Properties:
          ApiId: !Ref BattleshipHttpApi
          Path: /api/rooms
          Method: POST
```

#### Bước 1: Build mã nguồn Backend

Di chuyển vào thư mục `BackEnd/` và chạy lệnh build:

```bash
cd BackEnd
sam build
```

SAM sẽ tải dependencies của Node.js và đóng gói từng hàm Lambda thành artifact riêng biệt.

#### Bước 2: Deploy lần đầu (chế độ tương tác)

```bash
sam deploy --guided
```

SAM CLI sẽ hỏi bạn các thông số sau:

```
Stack Name [sam-app]: cloud-battleship-backend
AWS Region [us-east-1]: ap-southeast-1
Parameter CognitoUserPoolId []: <USER_POOL_ID_từ_bước_5.2>
Parameter CognitoUserPoolClientId []: <CLIENT_ID_từ_bước_5.2>
Parameter CorsOrigin [http://localhost:5173]: http://localhost:5173
Confirm changes before deploy [y/N]: y
Allow SAM CLI IAM role creation [Y/n]: Y
Save arguments to configuration file [Y/n]: Y
```

Sau khoảng 3–5 phút, stack được tạo thành công.

#### Bước 3: Lấy thông số Output

Sau khi deploy xong, terminal hiển thị phần **Outputs**. Ghi lại các giá trị sau:

```
Key                 HttpApiUrl
Value               https://xxxxxxxxxx.execute-api.ap-southeast-1.amazonaws.com

Key                 WebSocketUrl
Value               wss://yyyyyyyyyy.execute-api.ap-southeast-1.amazonaws.com/Prod
```

#### Bước 4: Kiểm tra API hoạt động

Thử gọi HTTP API bằng `curl` hoặc công cụ như Postman:

```bash
# Tạo một phòng mới (cần JWT token từ Cognito)
curl -X POST https://<HttpApiUrl>/api/rooms \
  -H "Authorization: Bearer <JWT_TOKEN>" \
  -H "Content-Type: application/json"
```

Kết quả mong đợi:
```json
{
  "roomCode": "ABC123",
  "createdAt": "2026-07-09T06:00:00.000Z"
}
```

#### Nội dung

- [Tạo và cấu hình SAM stack](5.3.1-SAM-Build/)
- [Kiểm thử HTTP API](5.3.2-Test-HTTP-API/)