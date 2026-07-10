---
title : "Integrate Real-time WebSocket"
date : 2024-01-01 
weight : 4
chapter : false
pre : " <b> 5.4. </b> "
---

### Goals

Verify that the real-time WebSocket channel is correctly deployed by checking the route configuration in the AWS Console and performing a live connection test using `wscat` from the terminal.

---

#### WebSocket vs HTTP — Core Differences

| Feature | HTTP | WebSocket |
|---|---|---|
| Connection type | Request → Response (one-way) | Persistent connection (bi-directional) |
| Who sends first | Always the Client | Both Client and Server |
| Best suited for | REST API, data fetching | Real-time games, chat, notifications |
| Used in project for | Room creation, user profiles | Shots fired, chat, match end events |

---

#### Verify Configuration (AWS Console)

1. Navigate to **AWS Console → API Gateway → APIs**.
2. Select **cloud-battleship-backend-dev-WebSocketApi**.
3. Open the **Routes** panel from the left navigation.
4. Verify that the 3 default routes are correctly registered:
   - `$connect` (integrated with `wsConnect` Lambda)
   - `$disconnect` (integrated with `wsDisconnect` Lambda)
   - `$default` (integrated with `wsMessage` Lambda)

![API Gateway WebSocket Routes](/images/5-Workshop/5.4-WebSocket-Realtime/api-gateway-websocket-routes.png)

---

#### Test a Live WebSocket Connection with `wscat`

`wscat` is a CLI tool that lets you send and receive WebSocket messages directly from your terminal without a browser.

**Step 1: Install wscat**

```bash
npm install -g wscat
```

**Step 2: Connect to the WebSocket API**

Use the `WebSocketUrl` from the SAM Outputs in step 5.3:

```bash
wscat -c wss://<YOUR_WEBSOCKET_API_ID>.execute-api.ap-southeast-1.amazonaws.com/prod
```

A successful connection displays the interactive prompt:
```
Connected (press CTRL+C to quit)
>
```

**Step 3: Send a test message**

Type and send the following payload:

```json
{"action": "ping"}
```

The `wsMessage` Lambda will handle the event. The key confirmation is that the connection stays open and Lambda is invoked — visible in CloudWatch Logs.

{{% notice tip %}}
If you receive a `403 Forbidden` error, check the IAM policy of the `wsConnect` Lambda — it requires `execute-api:ManageConnections` permission to send messages back to the client.
{{% /notice %}}

---

#### Confirm Entry in DynamoDB (ConnectionsTable)

Once `wscat` connects successfully, a new record is written to the `Connections` table:

1. Navigate to **AWS Console → DynamoDB → Tables → Connections** (actual name follows the pattern `cloud-battleship-backend-dev-ConnectionsTable-...`).
2. Select **Explore table items**.
3. Verify that a new item appears with a `connectionId` matching your active `wscat` session.
4. Press `Ctrl+C` to disconnect `wscat`, then refresh DynamoDB — confirm the item has been deleted by the `wsDisconnect` Lambda.
