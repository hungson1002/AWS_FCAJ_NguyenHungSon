---
title : "Integrate Real-time WebSocket"
date : 2024-01-01 
weight : 4
chapter : false
pre : " <b> 5.4. </b> "
---

### Goals

Configure API Gateway WebSocket integration to maintain persistent bi-directional communication channels for matchmaking and gameplay actions.

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
