---
title : "Tích hợp WebSocket real-time"
date : 2024-01-01 
weight : 4 
chapter : false
pre : " <b> 5.4. </b> "
---

#### Tổng quan về WebSocket trong game real-time

**WebSocket** là giao thức kết nối hai chiều (bidirectional) liên tục giữa client và server. Khác với HTTP (request-response), WebSocket duy trì một kết nối mở để server có thể chủ động đẩy dữ liệu xuống client bất cứ lúc nào.

Trong **Cloud Battleship Arena**, WebSocket được dùng để:
+ **Đồng bộ xếp tàu**: Khi cả hai người chơi đã sẵn sàng (`LobbyReady`), server broadcast tín hiệu bắt đầu.
+ **Truyền tọa độ bắn**: Người chơi gửi `FIRE` với tọa độ, server broadcast cho đối thủ để kiểm tra trúng/trượt.
+ **Chat trong phòng**: Tin nhắn chat được relay qua WebSocket với độ trễ thấp.
+ **Thông báo kết thúc trận**: Khi tàu cuối cùng bị chìm, server gửi `GAME_OVER` payload.

#### Tại sao dùng API Gateway WebSocket thay vì tự quản lý server?

+ **Không cần quản lý kết nối**: API Gateway tự động xử lý handshake, giữ kết nối sống, và route message.
+ **Tích hợp với Lambda**: Mỗi sự kiện WebSocket kích hoạt một hàm Lambda riêng biệt.
+ **TTL tự động**: Các `connectionId` stale được dọn dẹp tự động qua DynamoDB TTL.

#### Luồng hoạt động WebSocket

```
1. Client kết nối: wss://<WebSocketUrl>?token=<JWT>
       ↓
2. API Gateway kiểm tra token và kích hoạt wsConnect Lambda
       ↓
3. wsConnect lưu {connectionId, roomCode, userId} vào ConnectionsTable
       ↓
4. Client gửi message: {"action": "FIRE", "row": 3, "col": 5}
       ↓
5. API Gateway route theo "action" → kích hoạt wsMessage Lambda
       ↓
6. wsMessage truy vấn ConnectionsTable để tìm connectionId của đối thủ
       ↓
7. wsMessage gọi API Gateway Management API để gửi kết quả cho đối thủ
       ↓
8. Client ngắt kết nối → wsDisconnect Lambda dọn dẹp record trong ConnectionsTable
```

#### Cấu hình WebSocket trong template.yaml

```yaml
BattleshipWebSocketApi:
  Type: AWS::ApiGatewayV2::Api
  Properties:
    Name: CloudBattleshipWebSocket
    ProtocolType: WEBSOCKET
    RouteSelectionExpression: "$request.body.action"

WebSocketConnectFunction:
  Type: AWS::Serverless::Function
  Properties:
    Handler: src/handlers/wsConnect.handler
    Events:
      Connect:
        Type: HttpApi   # WebSocket $connect route
```

#### Bước 1: Kiểm thử WebSocket bằng wscat

Cài đặt công cụ `wscat` để test WebSocket từ terminal:

```bash
npm install -g wscat
```

Kết nối tới WebSocket API:
```bash
wscat -c "wss://<WebSocketUrl>?roomCode=ABC123" \
  -H "Authorization: Bearer <JWT_TOKEN>"
```

#### Bước 2: Gửi hành động game

Sau khi kết nối thành công, gửi một message:
```json
{"action": "FIRE", "row": 3, "col": 5}
```

Đối thủ trong cùng phòng sẽ nhận được broadcast:
```json
{
  "type": "OPPONENT_FIRE",
  "row": 3,
  "col": 5,
  "result": "HIT"
}
```

#### Bước 3: Kiểm tra ConnectionsTable trên DynamoDB Console

Mở AWS Console → DynamoDB → Tables → `Connections`. Bạn sẽ thấy record của kết nối vừa tạo với các field:
- `connectionId`: ID duy nhất được API Gateway cấp
- `roomCode`: Mã phòng
- `userId`: ID người chơi từ Cognito
- `ttl`: Thời gian sống (Unix timestamp)

#### Nội dung

- [Chuẩn bị môi trường test WebSocket](5.4.1-WebSocket-Setup/)
- [Tạo Interface WebSocket API](5.4.2-Create-WebSocket-API/)
- [Kiểm thử kết nối real-time](5.4.3-Test-WebSocket/)
- [Mô phỏng luồng game đầy đủ](5.4.4-Game-Flow/)
