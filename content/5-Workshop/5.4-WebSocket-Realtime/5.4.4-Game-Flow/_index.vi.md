---
title : "Mô phỏng luồng game đầy đủ"
date : 2024-01-01
weight : 4
chapter : false
pre : " <b> 5.4.4 </b> "
---

#### Luồng game hoàn chỉnh từ đầu đến cuối

Trong phần này, bạn sẽ chạy toàn bộ vòng đời của một trận đấu Battleship — từ tạo phòng, kết nối WebSocket, xếp tàu, chiến đấu đến kết thúc trận.

#### Giai đoạn 1: Lobby (Sảnh)

```bash
# Player 1 tạo phòng
curl -X POST https://<HttpApiUrl>/api/rooms \
  -H "Authorization: Bearer <JWT_P1>" | jq .
# → {"roomCode": "XK9P2M"}

# Player 2 join phòng
curl -X POST https://<HttpApiUrl>/api/rooms/XK9P2M/join \
  -H "Authorization: Bearer <JWT_P2>"
```

#### Giai đoạn 2: Kết nối WebSocket

```bash
# Terminal 1 - Player 1
wscat -c "wss://<WebSocketUrl>?roomCode=XK9P2M"

# Terminal 2 - Player 2  
wscat -c "wss://<WebSocketUrl>?roomCode=XK9P2M"
```

Cả hai nhận thông báo:
```json
{"type": "PLAYER_JOINED", "playersReady": 2}
```

#### Giai đoạn 3: Xếp tàu & Sẵn sàng

```bash
# Cả hai terminal gửi LobbyReady
{"action": "LobbyReady", "roomCode": "XK9P2M"}
```

Server broadcast:
```json
{"type": "GAME_START", "firstTurn": "player1"}
```

#### Giai đoạn 4: Chiến đấu (Combat)

```bash
# Player 1 bắn
{"action": "FIRE", "row": 2, "col": 3}

# Player 2 nhận
{"type": "OPPONENT_FIRE", "row": 2, "col": 3, "result": "HIT"}

# Player 2 bắn
{"action": "FIRE", "row": 9, "col": 9}

# Player 1 nhận
{"type": "OPPONENT_FIRE", "row": 9, "col": 9, "result": "MISS"}
```

#### Giai đoạn 5: Kết thúc trận (Game Over)

Khi tàu cuối cùng của một người chơi bị chìm, server gửi:

```json
{
  "type": "GAME_OVER",
  "winner": "player1",
  "rpChange": +25,
  "newRankPoints": 1225
}
```

#### Kiểm tra MatchHistory trong DynamoDB

Mở **AWS Console → DynamoDB → Tables → MatchHistory**:

```
matchId:   "match-xk9p2m-1720500000"
roomCode:  "XK9P2M"
winner:    "player1"
loser:     "player2"
duration:  342  (seconds)
timestamp: "2026-07-09T06:00:00Z"
```

#### Kiểm tra cập nhật điểm Rank

Mở **DynamoDB → Tables → User** và xác nhận `rankPoints` của Player 1 đã tăng.

#### Tóm tắt

Chúc mừng! Bạn đã hoàn thành mô phỏng toàn bộ vòng đời của một trận Battleship trên AWS Serverless:

| Giai đoạn | Dịch vụ AWS | Lambda handler |
|---|---|---|
| Tạo phòng | API Gateway HTTP | `createRoom` |
| Join phòng | API Gateway HTTP | `joinRoom` |
| Kết nối game | API Gateway WebSocket | `wsConnect` |
| Xếp tàu sẵn sàng | API Gateway WebSocket | `wsMessage` |
| Bắn đạn | API Gateway WebSocket | `wsMessage` |
| Kết thúc | API Gateway WebSocket | `wsMessage` |
| Lưu lịch sử | DynamoDB | (trong `wsMessage`) |
| Ngắt kết nối | API Gateway WebSocket | `wsDisconnect` |