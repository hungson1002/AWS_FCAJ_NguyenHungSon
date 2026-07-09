---
title : "Connect to WebSocket API"
date : 2024-01-01
weight : 2
chapter : false
pre : " <b> 5.4.2 </b> "
---

#### Connect Both Players to the Same Room

Open 2 terminals and run simultaneously:

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

If the connection is successful, you'll see a `>` prompt in both terminals.

#### Verify Connections in DynamoDB

Open **AWS Console → DynamoDB → Tables → Connections**. You will see 2 new records:

| connectionId | roomCode | userId |
|---|---|---|
| `aB1cD2eF3g=` | `XK9P2M` | `player1` |
| `hI4jK5lM6n=` | `XK9P2M` | `player2` |

#### Signal Ready (LobbyReady)

When both players have placed their ships and are ready to fight, send the ready signal from each terminal:

**Terminal 1:**
```json
{"action": "LobbyReady", "roomCode": "XK9P2M"}
```

**Terminal 2:**
```json
{"action": "LobbyReady", "roomCode": "XK9P2M"}
```

When both players send `LobbyReady`, the server broadcasts to both:
```json
{"type": "GAME_START", "firstTurn": "player1"}
```

{{% notice note %}}
The `wsMessage` Lambda uses the **API Gateway Management API** to send messages back to clients via `connectionId`. This is how API Gateway WebSocket enables server-initiated messages.
{{% /notice %}}