---
title : "Chuẩn bị môi trường test WebSocket"
date : 2024-01-01
weight : 1
chapter : false
pre : " <b> 5.4.1 </b> "
---

#### Tại sao cần môi trường test riêng cho WebSocket?

WebSocket khác HTTP ở chỗ nó là kết nối liên tục — bạn không thể test bằng `curl` thông thường. Bạn cần công cụ chuyên dụng để:
+ Khởi tạo và duy trì kết nối WebSocket.
+ Gửi message theo định dạng JSON.
+ Nhận và hiển thị message từ server.

#### Bước 1: Cài đặt wscat

**wscat** là CLI tool phổ biến nhất để test WebSocket:

```bash
npm install -g wscat
```

Kiểm tra cài đặt:
```bash
wscat --version
```

#### Bước 2: Lấy WebSocket URL

Từ output của lệnh `sam deploy`, lấy `WebSocketUrl`:

```
Key      WebSocketUrl
Value    wss://abc123xyz.execute-api.ap-southeast-1.amazonaws.com/Prod
```

#### Bước 3: Tạo phòng để test

Trước khi kết nối WebSocket, tạo một phòng qua HTTP API:

```bash
ROOM_CODE=$(curl -s -X POST https://<HttpApiUrl>/api/rooms \
  -H "Authorization: Bearer <JWT_TOKEN>" \
  -H "Content-Type: application/json" | jq -r '.roomCode')

echo "Room Code: $ROOM_CODE"
```

#### Bước 4: Chuẩn bị 2 terminal window

WebSocket game yêu cầu ít nhất 2 người chơi trong cùng phòng. Mở **2 terminal windows** riêng biệt:

+ **Terminal 1**: Người chơi 1 (Player 1) — dùng JWT token thứ nhất.
+ **Terminal 2**: Người chơi 2 (Player 2) — tạo thêm một Cognito user thứ hai và dùng JWT token thứ hai.

```bash
# Trong Terminal 2 - Tạo user thứ 2
aws cognito-idp sign-up \
  --client-id <CLIENT_ID> \
  --username player2@example.com \
  --password "Test@12345" \
  --region ap-southeast-1

aws cognito-idp admin-confirm-sign-up \
  --user-pool-id <USER_POOL_ID> \
  --username player2@example.com \
  --region ap-southeast-1
```

{{% notice info %}}
Trong thực tế, hai người chơi sẽ dùng hai tài khoản Cognito khác nhau. Trong workshop này, bạn có thể dùng 1 tài khoản test hoặc 2 tài khoản để mô phỏng đầy đủ.
{{% /notice %}}
