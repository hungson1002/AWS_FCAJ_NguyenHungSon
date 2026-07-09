---
title : "Simulate Full Game Flow"
date : 2024-01-01
weight : 4
chapter : false
pre : " <b> 5.4.4 </b> "
---

#### Complete Game Lifecycle End-to-End

In this section, you will run through the complete lifecycle of a Battleship match — from creating a room, connecting via WebSocket, placing ships, fighting, to the game ending.

#### Phase 1: Lobby

```bash
# Player 1 creates a room
curl -X POST https://<HttpApiUrl>/api/rooms \
  -H "Authorization: Bearer <JWT_P1>" | jq .
# → {"roomCode": "XK9P2M"}

# Player 2 joins the room
curl -X POST https://<HttpApiUrl>/api/rooms/XK9P2M/join \
  -H "Authorization: Bearer <JWT_P2>"
```

#### Phase 2: WebSocket Connection

```bash
# Terminal 1 - Player 1
wscat -c "wss://<WebSocketUrl>?roomCode=XK9P2M"

# Terminal 2 - Player 2
wscat -c "wss://<WebSocketUrl>?roomCode=XK9P2M"
```

Both receive a notification:
```json
{"type": "PLAYER_JOINED", "playersReady": 2}
```

#### Phase 3: Ship Placement & Ready

```bash
# Both terminals send LobbyReady
{"action": "LobbyReady", "roomCode": "XK9P2M"}
```

Server broadcasts:
```json
{"type": "GAME_START", "firstTurn": "player1"}
```

#### Phase 4: Combat

```bash
# Player 1 fires
{"action": "FIRE", "row": 2, "col": 3}

# Player 2 receives
{"type": "OPPONENT_FIRE", "row": 2, "col": 3, "result": "HIT"}

# Player 2 fires back
{"action": "FIRE", "row": 9, "col": 9}

# Player 1 receives
{"type": "OPPONENT_FIRE", "row": 9, "col": 9, "result": "MISS"}
```

#### Phase 5: Game Over

When the last ship of a player sinks, the server sends:

```json
{
  "type": "GAME_OVER",
  "winner": "player1",
  "rpChange": +25,
  "newRankPoints": 1225
}
```

#### Verify MatchHistory in DynamoDB

Open **AWS Console → DynamoDB → Tables → MatchHistory**:

```
matchId:   "match-xk9p2m-1720500000"
roomCode:  "XK9P2M"
winner:    "player1"
loser:     "player2"
duration:  342  (seconds)
timestamp: "2026-07-09T06:00:00Z"
```

#### Verify Rank Points Update

Open **DynamoDB → Tables → User** and confirm Player 1's `rankPoints` has increased.

#### Summary

Congratulations! You have completed a simulation of the full lifecycle of a Battleship match on AWS Serverless:

| Phase | AWS Service | Lambda Handler |
|---|---|---|
| Create room | API Gateway HTTP | `createRoom` |
| Join room | API Gateway HTTP | `joinRoom` |
| Game connect | API Gateway WebSocket | `wsConnect` |
| Ship placement ready | API Gateway WebSocket | `wsMessage` |
| Fire shots | API Gateway WebSocket | `wsMessage` |
| Game over | API Gateway WebSocket | `wsMessage` |
| Save match history | DynamoDB | (inside `wsMessage`) |
| Disconnect cleanup | API Gateway WebSocket | `wsDisconnect` |