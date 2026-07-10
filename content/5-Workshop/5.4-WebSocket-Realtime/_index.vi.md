---
title : "Tích hợp WebSocket real-time"
date : 2024-01-01 
weight : 4
chapter : false
pre : " <b> 5.4. </b> "
---

### Mục tiêu

Cấu hình tích hợp API Gateway WebSocket để duy trì kênh giao tiếp hai chiều thời gian thực phục vụ quá trình ghép trận và bắn tàu.

---

#### WebSocket vs HTTP — Sự khác biệt cốt lõi

| Đặc tính | HTTP | WebSocket |
|---|---|---|
| Kiểu kết nối | Request → Response (1 chiều) | Kết nối liên tục (2 chiều) |
| Ai gửi trước | Luôn là Client | Cả Client lẫn Server |
| Phù hợp cho | REST API, load data | Game real-time, chat, notifications |
| Trong dự án | Tạo phòng, hồ sơ | Bắn đạn, chat, kết thúc trận |

---

#### Kiểm tra kết quả (AWS Console)

1. Mở **AWS Console → API Gateway → APIs**.
2. Click chọn **cloud-battleship-backend-dev-WebSocketApi**.
3. Chọn tab **Routes** ở thanh menu bên trái.
4. Xác nhận hệ thống đã khởi tạo đủ 3 route mặc định:
   - `$connect` (trỏ về Lambda `wsConnect`)
   - `$disconnect` (trỏ về Lambda `wsDisconnect`)
   - `$default` (trỏ về Lambda `wsMessage`)

![API Gateway WebSocket Routes](/images/5-Workshop/5.4-WebSocket-Realtime/api-gateway-websocket-routes.png)
