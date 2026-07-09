---
title : "Bảo mật API với Cognito (nâng cao)"
date : 2024-01-01
weight : 5
chapter : false
pre : " <b> 5.5 </b> "
---

Khi bạn tạo một HTTP API với API Gateway, bạn có thể đính kèm một **Authorizer** để kiểm soát quyền truy cập. **Amazon Cognito JWT Authorizer** tích hợp trực tiếp với API Gateway, giúp xác thực token của người chơi mà không cần viết thêm code.

Bạn cũng có thể tạo các **IAM Policy** chi tiết để giới hạn quyền của Lambda theo nguyên tắc **Least Privilege** (đặc quyền tối thiểu) — mỗi hàm chỉ có quyền truy cập vào bảng DynamoDB mà nó thực sự cần.

Trong phần này, bạn sẽ kiểm tra luồng bảo mật của Cloud Battleship Arena và cấu hình một policy Lambda chi tiết hơn.

#### Bước 1: Xác minh Cognito Authorizer đang hoạt động

Thử gọi một API được bảo vệ mà **không** có token:

```bash
curl -X GET https://<HttpApiUrl>/api/users/me
```

Kết quả mong đợi (401 Unauthorized):
```json
{"message": "Unauthorized"}
```

Gọi lại **với** JWT token hợp lệ:
```bash
curl -X GET https://<HttpApiUrl>/api/users/me \
  -H "Authorization: Bearer <JWT_TOKEN>"
```

Kết quả mong đợi (200 OK):
```json
{
  "userId": "abc123",
  "username": "Commander_Son",
  "rankPoints": 1200
}
```

#### Bước 2: Kiểm tra Least Privilege Policy của Lambda

Trong `template.yaml`, mỗi Lambda function được gắn policy theo nguyên tắc least privilege:

```yaml
# GetUserFunction chỉ có quyền READ trên UserTable
GetUserFunction:
  Type: AWS::Serverless::Function
  Properties:
    Policies:
      - DynamoDBReadPolicy:
          TableName: !Ref UserTable

# CreateRoomFunction chỉ có quyền CRUD trên RoomsTable
CreateRoomFunction:
  Type: AWS::Serverless::Function
  Properties:
    Policies:
      - DynamoDBCrudPolicy:
          TableName: !Ref RoomsTable
```

Mở **AWS Console → IAM → Roles**, tìm role của Lambda `GetUser`. Xác nhận rằng nó chỉ có quyền `dynamodb:GetItem`, `dynamodb:Query`, `dynamodb:Scan` — không có quyền ghi.

#### Bước 3: Tạo một policy tùy chỉnh (nâng cao)

Bạn muốn giới hạn Lambda `wsMessage` chỉ được broadcast tới các connectionId trong cùng phòng. Tạo một Resource-based policy DynamoDB:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowRoomScopedRead",
      "Effect": "Allow",
      "Action": [
        "dynamodb:Query",
        "dynamodb:GetItem"
      ],
      "Resource": [
        "arn:aws:dynamodb:*:*:table/Connections",
        "arn:aws:dynamodb:*:*:table/Connections/index/byRoomCode"
      ]
    }
  ]
}
```

#### Bước 4: Kiểm tra policy hoạt động đúng

Thử gọi WebSocket từ một token thuộc phòng khác và xác nhận rằng Lambda từ chối broadcast:

```bash
# Kết nối với roomCode sai
wscat -c "wss://<WebSocketUrl>?roomCode=WRONG_ROOM"
# Gửi FIRE action → Server trả về lỗi 403
{"action": "FIRE", "row": 0, "col": 0}
```

Kết quả:
```json
{"error": "Forbidden: You are not in this room"}
```

Trong phần này, bạn đã:
- Xác minh Cognito JWT Authorizer bảo vệ HTTP API.
- Kiểm tra nguyên tắc Least Privilege trong IAM Role của Lambda.
- Tạo policy tùy chỉnh để kiểm soát phạm vi truy cập DynamoDB.
