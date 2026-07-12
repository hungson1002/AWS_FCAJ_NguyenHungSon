---
title : "Tích hợp WebSocket real-time"
date : 2026-07-10 
weight : 4
chapter : false
pre : " <b> 5.4. </b> "
---

### Mục tiêu

Xác nhận kênh WebSocket real-time hoạt động đúng bằng cách kiểm tra route cấu hình trên AWS Console và kiểm thử kết nối thực tế bằng công cụ `wscat` từ terminal.

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

---

#### Kiểm thử kết nối WebSocket bằng `wscat`

`wscat` là công cụ CLI cho phép gửi/nhận tin nhắn WebSocket trực tiếp từ terminal mà không cần giao diện.

**Bước 1: Cài đặt wscat**

```bash
npm install -g wscat
```

**Bước 2: Kết nối đến WebSocket API**

Lấy giá trị `WebSocketUrl` từ Outputs của SAM ở bước 5.3 và chạy:

```bash
wscat -c wss://<YOUR_WEBSOCKET_API_ID>.execute-api.ap-southeast-1.amazonaws.com/prod
```

Nếu kết nối thành công, terminal hiển thị dấu nhắc:
```
Connected (press CTRL+C to quit)
>
```

**Bước 3: Gửi một tin nhắn test**

Trong cửa sổ `wscat`, gửi payload sau:

```json
{"action": "ping"}
```

Lambda `wsMessage` sẽ xử lý và trả về phản hồi (hoặc không trả nếu chưa có route `ping`). Điều quan trọng là kết nối không bị đứt và Lambda được gọi.

{{% notice tip %}}
Nếu nhận được lỗi `403 Forbidden` khi `wscat` kết nối, kiểm tra cấu hình route `$connect` trên API Gateway — route này có thể đang bật IAM Authorization không cần thiết. Đảm bảo Authorization của route `$connect` được đặt là **NONE**. (Lưu ý: quyền `execute-api:ManageConnections` là quyền Lambda dùng để **gửi tin nhắn ngược về client**, không liên quan đến việc client kết nối vào.)
{{% /notice %}}

---

#### Xác nhận trong DynamoDB (ConnectionsTable)

Sau khi `wscat` kết nối thành công, bảng `Connections` trong DynamoDB sẽ có một record mới:

1. Mở **AWS Console → DynamoDB → Tables → Connections** (tên thực tế dạng `cloud-battleship-backend-dev-ConnectionsTable-...`).
2. Chọn **Explore table items**.
3. Xác nhận có một item với trường `connectionId` tương ứng với phiên `wscat` của bạn.
4. Nhấn `Ctrl+C` để ngắt kết nối `wscat`, sau đó làm mới DynamoDB — xác nhận item đó đã bị xóa bởi Lambda `wsDisconnect`.
