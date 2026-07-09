---
title : "Kiểm thử HTTP API"
date : 2024-01-01 
weight : 2
chapter : false
pre : " <b> 5.3.2 </b> "
---

#### Lấy JWT token từ Cognito để test

Trước khi test API, bạn cần JWT token từ Cognito. Đăng ký và đăng nhập:

```bash
# Đăng ký người dùng mới
aws cognito-idp sign-up \
  --client-id <CLIENT_ID> \
  --username testplayer@example.com \
  --password "Test@12345" \
  --region ap-southeast-1

# Xác nhận tài khoản (bỏ qua bước xác minh email trong môi trường test)
aws cognito-idp admin-confirm-sign-up \
  --user-pool-id <USER_POOL_ID> \
  --username testplayer@example.com \
  --region ap-southeast-1

# Đăng nhập để lấy token
aws cognito-idp initiate-auth \
  --client-id <CLIENT_ID> \
  --auth-flow USER_PASSWORD_AUTH \
  --auth-parameters USERNAME=testplayer@example.com,PASSWORD="Test@12345" \
  --region ap-southeast-1
```

Sao chép giá trị `IdToken` từ kết quả — đây là JWT token bạn sẽ dùng.

#### Kiểm thử từng endpoint HTTP API

**1. Tạo User Profile (POST /api/users)**

```bash
curl -X POST https://<HttpApiUrl>/api/users \
  -H "Authorization: Bearer <JWT_TOKEN>" \
  -H "Content-Type: application/json" \
  -d '{"username": "Commander_Son"}'
```

Kết quả mong đợi:
```json
{"userId": "abc123", "username": "Commander_Son", "rankPoints": 0}
```

**2. Tạo phòng mới (POST /api/rooms)**

```bash
curl -X POST https://<HttpApiUrl>/api/rooms \
  -H "Authorization: Bearer <JWT_TOKEN>" \
  -H "Content-Type: application/json"
```

Kết quả mong đợi:
```json
{"roomCode": "XK9P2M", "status": "waiting"}
```

**3. Tham gia phòng (POST /api/rooms/{roomCode}/join)**

```bash
curl -X POST https://<HttpApiUrl>/api/rooms/XK9P2M/join \
  -H "Authorization: Bearer <JWT_TOKEN>"
```

**4. Matchmaking tự động (POST /api/matchmaking)**

```bash
curl -X POST https://<HttpApiUrl>/api/matchmaking \
  -H "Authorization: Bearer <JWT_TOKEN>"
```

Kết quả (tìm được phòng trống hoặc tạo phòng mới):
```json
{"roomCode": "XK9P2M", "matched": true}
```

#### Kiểm tra DynamoDB Console

Sau khi gọi API, mở **AWS Console → DynamoDB → Tables → Rooms** và xác nhận record đã được tạo với các field:

- `roomCode` — Khóa chính (ví dụ: `XK9P2M`)
- `status` — Trạng thái phòng (`waiting` hoặc `full`)
- `ttl` — Thời gian tự xóa (Unix timestamp, thường là 1 giờ sau)

{{% notice note %}}
**TTL (Time To Live)** là tính năng của DynamoDB tự động xóa các item hết hạn. Trong Cloud Battleship Arena, các phòng bị bỏ hoang sẽ tự động xóa sau 1 giờ — không cần viết code dọn dẹp.
{{% /notice %}}

#### Tóm tắt

Chúc mừng! Bạn đã thành công deploy Backend bằng AWS SAM và kiểm thử các HTTP API endpoints. Trong phần tiếp theo, chúng ta sẽ kết nối WebSocket để trải nghiệm gameplay real-time.