---
title : "WebSocket Test Environment Setup"
date : 2024-01-01
weight : 1
chapter : false
pre : " <b> 5.4.1 </b> "
---

#### Why Do You Need a Dedicated WebSocket Test Environment?

WebSocket differs from HTTP in that it is a persistent connection — you can't test it with standard `curl`. You need a dedicated tool to:
+ Initiate and maintain a WebSocket connection.
+ Send messages in JSON format.
+ Receive and display messages from the server.

#### Step 1: Install wscat

**wscat** is the most popular CLI tool for testing WebSocket:

```bash
npm install -g wscat
```

Verify installation:
```bash
wscat --version
```

#### Step 2: Get the WebSocket URL

From the `sam deploy` output, note the `WebSocketUrl`:

```
Key      WebSocketUrl
Value    wss://abc123xyz.execute-api.ap-southeast-1.amazonaws.com/Prod
```

#### Step 3: Create a Room for Testing

Before connecting via WebSocket, create a room through the HTTP API:

```bash
ROOM_CODE=$(curl -s -X POST https://<HttpApiUrl>/api/rooms \
  -H "Authorization: Bearer <JWT_TOKEN>" \
  -H "Content-Type: application/json" | jq -r '.roomCode')

echo "Room Code: $ROOM_CODE"
```

#### Step 4: Prepare 2 Terminal Windows

A WebSocket game requires at least 2 players in the same room. Open **2 separate terminal windows**:

+ **Terminal 1**: Player 1 — uses the first JWT token.
+ **Terminal 2**: Player 2 — create a second Cognito user and use their JWT token.

```bash
# In Terminal 2 - Create the second user
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
In a real scenario, two players use two separate Cognito accounts. In this workshop, you can use 1 test account or 2 accounts to fully simulate the flow.
{{% /notice %}}
