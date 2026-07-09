---
title : "Kiểm thử kết nối WebSocket real-time"
date : 2024-01-01
weight : 3
chapter : false
pre : " <b> 5.4.3 </b> "
---

#### Mô phỏng hành động bắn trong game

Sau khi cả hai người chơi kết nối và nhận `GAME_START`, bắt đầu luân phiên bắn đạn.

**Player 1 bắn ô (3, 5):**

Trong Terminal 1, gõ:
```json
{"action": "FIRE", "row": 3, "col": 5}
```

**Player 2 nhận broadcast ngay lập tức:**

Terminal 2 sẽ hiển thị:
```json
{"type": "OPPONENT_FIRE", "row": 3, "col": 5, "result": "MISS"}
```

**Player 2 phản hồi kết quả và bắn lại:**
```json
{"action": "FIRE", "row": 0, "col": 0}
```

**Player 1 nhận:**
```json
{"type": "OPPONENT_FIRE", "row": 0, "col": 0, "result": "HIT"}
```

#### Kiểm tra CloudWatch Logs

Sau khi gửi messages, xem logs của Lambda `wsMessage` để hiểu luồng xử lý:

```bash
aws logs tail /aws/lambda/WebSocketMessage \
  --follow \
  --region ap-southeast-1
```

Bạn sẽ thấy logs tương tự:
```
[INFO] Received action: FIRE from connectionId: aB1cD2eF3g=
[INFO] Looking up opponent connectionId for roomCode: XK9P2M
[INFO] Found opponent connectionId: hI4jK5lM6n=
[INFO] Broadcasting OPPONENT_FIRE to hI4jK5lM6n=
[INFO] Broadcast successful
```

#### Kiểm tra độ trễ (Latency)

Ghi lại thời gian từ lúc Player 1 gửi đến lúc Player 2 nhận. Với API Gateway WebSocket + Lambda, độ trễ thường dưới **100ms** trong cùng Region.

{{% notice tip %}}
Để giảm thiểu độ trễ Lambda cold start, bạn có thể bật **Provisioned Concurrency** trong `template.yaml`. Tuy nhiên, điều này sẽ tăng chi phí — phù hợp với môi trường production.
{{% /notice %}}
