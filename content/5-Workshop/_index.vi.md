---
title: "Workshop"
date: 2024-01-01
weight: 5
chapter: false
pre: " <b> 5. </b> "
---

# Xây dựng Backend Serverless cho Cloud Battleship Arena

#### Tổng quan

**AWS Serverless** cho phép bạn chạy các ứng dụng mà không cần quản lý máy chủ — AWS tự động mở rộng theo nhu cầu và bạn chỉ trả tiền cho những gì thực sự sử dụng.

Trong workshop này, chúng ta sẽ học cách thiết kế, triển khai và kiểm thử một hệ thống backend hoàn chỉnh cho trò chơi bắn tàu thời gian thực đa người chơi (**Cloud Battleship Arena**) bằng cách sử dụng **AWS SAM**, **AWS Lambda**, **Amazon API Gateway** (HTTP & WebSocket), **Amazon DynamoDB**, và **Amazon Cognito**.

Chúng ta sẽ xây dựng hai loại API:
+ **HTTP API (REST)** — Để quản lý phòng chơi, hồ sơ người dùng và matchmaking.
+ **WebSocket API** — Để truyền tải trạng thái game theo thời gian thực giữa các người chơi.

#### Nội dung

1. [Tổng quan về workshop](5.1-Workshop-overview/)
2. [Chuẩn bị môi trường](5.2-Prerequiste/)
3. [Triển khai Backend với AWS SAM](5.3-SAM-Backend/)
4. [Tích hợp WebSocket real-time](5.4-WebSocket-Realtime/)
5. [Bảo mật API với Cognito (nâng cao)](5.5-Cognito-Security/)
6. [Dọn dẹp tài nguyên](5.6-Cleanup/)