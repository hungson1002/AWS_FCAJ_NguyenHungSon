---
title : "Cấu hình và Build SAM Stack"
date : 2024-01-01 
weight : 1
chapter : false
pre : " <b> 5.3.1 </b> "
---

### Mục tiêu

Hiểu cấu trúc `template.yaml` và đóng gói thành công toàn bộ Backend thành artifact sẵn sàng để deploy lên AWS.

---

#### Cấu trúc tài nguyên trong template.yaml

File `BackEnd/template.yaml` định nghĩa toàn bộ Backend gồm:
- **Globals**: Runtime `nodejs24.x`, Timeout 10 giây, biến môi trường cho cả 8 Lambda.
- **BattleshipHttpApi**: HTTP API Gateway với JWT Authorizer tích hợp Cognito.
- **BattleshipWebSocketApi**: WebSocket API Gateway với route selection theo field `action`.
- **RoomsTable / ConnectionsTable / ChatMessagesTable**: 3 bảng DynamoDB với TTL và GSI.

---

#### Bước 1: Cài đặt dependencies

```bash
cd AWS_Cloud_Battleship_Arena/BackEnd
npm install
```

---

#### Bước 2: Build SAM artifact

```bash
sam build
```

SAM đọc `template.yaml`, chạy `npm install` cho mỗi Lambda và đóng gói vào `.aws-sam/build/`.

---

#### Bước 3: Validate template (tùy chọn)

```bash
sam validate --lint
```

---

#### Kiểm tra kết quả build

```bash
ls .aws-sam/build/
# Kết quả: CreateRoomFunction/ JoinRoomFunction/ WebSocketConnectFunction/ ...
```

Mỗi thư mục tương ứng với một Lambda function đã được đóng gói riêng biệt.

{{% notice tip %}}
Sau lần đầu chạy `sam deploy --guided`, SAM tự lưu tham số vào `samconfig.toml`. Các lần deploy tiếp theo chỉ cần `sam deploy` — không cần `--guided`.
{{% /notice %}}
