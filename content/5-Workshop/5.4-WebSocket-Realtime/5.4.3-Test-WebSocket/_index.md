---
title : "Test Real-time WebSocket Connection"
date : 2024-01-01
weight : 3
chapter : false
pre : " <b> 5.4.3 </b> "
---

#### Simulate Firing Actions in the Game

After both players connect and receive `GAME_START`, begin taking turns firing shots.

**Player 1 fires at cell (3, 5):**

In Terminal 1, type:
```json
{"action": "FIRE", "row": 3, "col": 5}
```

**Player 2 receives the broadcast immediately:**

Terminal 2 will display:
```json
{"type": "OPPONENT_FIRE", "row": 3, "col": 5, "result": "MISS"}
```

**Player 2 fires back:**
```json
{"action": "FIRE", "row": 0, "col": 0}
```

**Player 1 receives:**
```json
{"type": "OPPONENT_FIRE", "row": 0, "col": 0, "result": "HIT"}
```

#### Check CloudWatch Logs

After sending messages, view the `wsMessage` Lambda logs to understand the processing flow:

```bash
aws logs tail /aws/lambda/WebSocketMessage \
  --follow \
  --region ap-southeast-1
```

You will see logs similar to:
```
[INFO] Received action: FIRE from connectionId: aB1cD2eF3g=
[INFO] Looking up opponent connectionId for roomCode: XK9P2M
[INFO] Found opponent connectionId: hI4jK5lM6n=
[INFO] Broadcasting OPPONENT_FIRE to hI4jK5lM6n=
[INFO] Broadcast successful
```

#### Measure Latency

Record the time from when Player 1 sends to when Player 2 receives. With API Gateway WebSocket + Lambda, latency is typically under **100ms** within the same Region.

{{% notice tip %}}
To minimize Lambda cold start latency, you can enable **Provisioned Concurrency** in `template.yaml`. However, this will increase costs — best suited for production environments.
{{% /notice %}}
