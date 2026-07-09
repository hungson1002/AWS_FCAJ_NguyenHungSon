---
title : "Giới thiệu"
date : 2024-01-01 
weight : 1
chapter : false
pre : " <b> 5.1. </b> "
---

#### Giới thiệu về kiến trúc Serverless Event-Driven

**Serverless** không có nghĩa là "không có máy chủ" — mà có nghĩa là bạn không cần quản lý hay duy trì máy chủ. AWS sẽ lo phần hạ tầng để bạn tập trung vào logic nghiệp vụ.

Kiến trúc **Event-driven** (hướng sự kiện) là nền tảng của Cloud Battleship Arena:
+ Khi người chơi **kết nối** → API Gateway WebSocket kích hoạt Lambda `wsConnect`.
+ Khi người chơi **bắn đạn** → Lambda `wsMessage` nhận tọa độ, tính toán và broadcast kết quả cho đối thủ.
+ Khi trận đấu **kết thúc** → Lambda cập nhật `MatchHistory` và điểm ELO trong DynamoDB.

#### Tổng quan về workshop

Trong workshop này, bạn sẽ xây dựng toàn bộ backend của **Cloud Battleship Arena** gồm hai phần chính:

+ **HTTP API** — Xử lý các thao tác RESTful: tạo phòng (`createRoom`), tham gia phòng (`joinRoom`), ghép trận tự động (`matchmakeRoom`), quản lý hồ sơ người dùng.
+ **WebSocket API** — Quản lý kết nối thời gian thực: bắt tay kết nối (`wsConnect`), định tuyến hành động game (`wsMessage`), dọn dẹp khi ngắt kết nối (`wsDisconnect`).

Toàn bộ hạ tầng được định nghĩa dưới dạng mã (IaC) bằng **AWS SAM** (`template.yaml`) và có thể triển khai lên AWS chỉ với một lệnh duy nhất.

#### Kiến trúc tổng quan

```
[React Frontend]
      |
      |-- HTTP Requests --> [API Gateway HTTP API] --> [Lambda Functions] --> [DynamoDB]
      |
      |-- WebSocket ------> [API Gateway WebSocket API] --> [Lambda wsConnect/wsMessage/wsDisconnect]
                                                                    |
                                                              [DynamoDB Connections/Rooms]
```

**Các dịch vụ AWS trong workshop:**

| Dịch vụ | Vai trò trong workshop |
|---|---|
| **AWS SAM** | Định nghĩa và triển khai toàn bộ hạ tầng |
| **AWS Lambda** | Xử lý logic game và API |
| **Amazon API Gateway (HTTP)** | REST endpoints cho room & user management |
| **Amazon API Gateway (WebSocket)** | Real-time channel cho gameplay |
| **Amazon DynamoDB** | Lưu trạng thái phòng, kết nối, lịch sử |
| **Amazon Cognito** | Xác thực người chơi |
| **Amazon S3** | Lưu trữ avatar người chơi |
