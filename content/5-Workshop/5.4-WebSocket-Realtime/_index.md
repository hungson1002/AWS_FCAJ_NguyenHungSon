---
title : "Integrate Real-time WebSocket"
date : 2024-01-01
weight : 4
chapter : false
pre : " <b> 5.4. </b> "
---

#### Overview of WebSocket in Real-time Games

**WebSocket** is a persistent bidirectional connection protocol between client and server. Unlike HTTP (request-response), WebSocket maintains an open connection so the server can push data to clients at any time.

In **Cloud Battleship Arena**, WebSocket is used to:
+ **Sync ship placement**: When both players are ready (`LobbyReady`), the server broadcasts the game start signal.
+ **Transmit firing coordinates**: A player sends `FIRE` with coordinates, the server broadcasts to the opponent to check hit/miss.
+ **In-room chat**: Chat messages are relayed via WebSocket with low latency.
+ **Game over notification**: When the last ship sinks, the server sends a `GAME_OVER` payload.

#### Why Use API Gateway WebSocket Instead of Managing Your Own Server?

+ **No connection management**: API Gateway automatically handles handshakes, keeps connections alive, and routes messages.
+ **Lambda integration**: Each WebSocket event triggers a separate Lambda function.
+ **Automatic TTL cleanup**: Stale `connectionId` records are cleaned up automatically via DynamoDB TTL.

#### WebSocket Flow

```
1. Client connects: wss://<WebSocketUrl>?token=<JWT>
       ↓
2. API Gateway validates token and triggers wsConnect Lambda
       ↓
3. wsConnect saves {connectionId, roomCode, userId} to ConnectionsTable
       ↓
4. Client sends message: {"action": "FIRE", "row": 3, "col": 5}
       ↓
5. API Gateway routes by "action" → triggers wsMessage Lambda
       ↓
6. wsMessage queries ConnectionsTable to find the opponent's connectionId
       ↓
7. wsMessage calls the API Gateway Management API to send results to the opponent
       ↓
8. Client disconnects → wsDisconnect Lambda cleans up the record in ConnectionsTable
```

#### WebSocket Configuration in template.yaml

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

#### Step 1: Test WebSocket Using wscat

Install `wscat` to test WebSocket from the terminal:

```bash
npm install -g wscat
```

Connect to the WebSocket API:
```bash
wscat -c "wss://<WebSocketUrl>?roomCode=ABC123" \
  -H "Authorization: Bearer <JWT_TOKEN>"
```

#### Step 2: Send a Game Action

After successfully connecting, send a message:
```json
{"action": "FIRE", "row": 3, "col": 5}
```

The opponent in the same room will receive the broadcast:
```json
{
  "type": "OPPONENT_FIRE",
  "row": 3,
  "col": 5,
  "result": "HIT"
}
```

#### Step 3: Check ConnectionsTable in DynamoDB Console

Open AWS Console → DynamoDB → Tables → `Connections`. You will see the record for the connection you just created with the following fields:
- `connectionId`: Unique ID assigned by API Gateway
- `roomCode`: Room code
- `userId`: Player ID from Cognito
- `ttl`: Time to live (Unix timestamp)

#### Content

- [Prepare WebSocket test environment](5.4.1-WebSocket-Setup/)
- [Create WebSocket API Interface](5.4.2-Create-WebSocket-API/)
- [Test real-time connection](5.4.3-Test-WebSocket/)
- [Simulate full game flow](5.4.4-Game-Flow/)
