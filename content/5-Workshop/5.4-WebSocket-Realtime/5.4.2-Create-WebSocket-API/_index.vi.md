---
title : "Kết nối WebSocket API"
date : 2024-01-01
weight : 2
chapter : false
pre : " <b> 5.4.2 </b> "
---

#### Kết nối cả hai người chơi vào cùng phòng

Mở 2 terminal và thực hiện đồng thời:

**Terminal 1 (Player 1):**
```bash
wscat -c "wss://<WebSocketUrl>?roomCode=<ROOM_CODE>&userId=player1" \
  --header "Authorization: Bearer <JWT_TOKEN_PLAYER1>"
```

**Terminal 2 (Player 2):**
```bash
wscat -c "wss://<WebSocketUrl>?roomCode=<ROOM_CODE>&userId=player2" \
  --header "Authorization: Bearer <JWT_TOKEN_PLAYER2>"
```

Nếu kết nối thành công, bạn thấy dấu nhắc `>` trong cả hai terminal.

#### Xác minh kết nối trong DynamoDB

Mở **AWS Console → DynamoDB → Tables → Connections**. Bạn sẽ thấy 2 record mới:

| connectionId | roomCode | userId |
|---|---|---|
| `aB1cD2eF3g=` | `XK9P2M` | `player1` |
| `hI4jK5lM6n=` | `XK9P2M` | `player2` |

#### Báo hiệu sẵn sàng (LobbyReady)

Khi cả hai người chơi đã xếp tàu xong và sẵn sàng chiến đấu, gửi signal từ mỗi terminal:

**Terminal 1:**
```json
{"action": "LobbyReady", "roomCode": "XK9P2M"}
```

**Terminal 2:**
```json
{"action": "LobbyReady", "roomCode": "XK9P2M"}
```

Khi cả hai đều gửi `LobbyReady`, server broadcast cho cả hai:
```json
{"type": "GAME_START", "firstTurn": "player1"}
```

{{% notice note %}}
Lambda `wsMessage` sử dụng **API Gateway Management API** để gửi message ngược lại cho client qua `connectionId`. Đây là cách API Gateway WebSocket cho phép server-initiated messages.
{{% /notice %}}