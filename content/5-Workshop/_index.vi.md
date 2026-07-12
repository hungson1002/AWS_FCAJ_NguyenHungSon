---
title: "Workshop"
date: 2026-07-10
weight: 5
chapter: false
pre: " <b> 5. </b> "
---

# Thực hành triển khai Cloud Battleship Arena lên AWS

#### Tổng quan

Trong workshop này, chúng ta sẽ đi qua toàn bộ quy trình thực tế để tự tay đóng gói, cấu hình và triển khai dự án game bắn tàu **Cloud Battleship Arena** lên hạ tầng điện toán đám mây **Amazon Web Services (AWS)**. 

Bài thực hành mô phỏng 100% các bước triển khai thực tế mà bạn đã thực hiện trong quá trình phát triển sản phẩm:
+ **Triển khai Backend Serverless** bằng AWS SAM (Lambda, API Gateway HTTP/WebSocket, DynamoDB).
+ **Triển khai Frontend** lưu trữ tĩnh trên Amazon S3 và phân phối toàn cầu qua CDN Amazon CloudFront.
+ **Thiết lập CI/CD tự động hóa** bằng GitHub Actions thông qua kết nối bảo mật OIDC (không cần dùng AWS Access Key/Secret Key dài hạn).

{{% notice info %}}
**Demo trực tiếp:** Bạn có thể trải nghiệm dự án đã được triển khai hoàn chỉnh ngay bây giờ trước khi bắt đầu workshop!
> **[https://d12bu86qwnw1gk.cloudfront.net/](https://d12bu86qwnw1gk.cloudfront.net/)**
{{% /notice %}}

#### Nội dung bài thực hành

1. [Giới thiệu](5.1-Introduction/)
2. [Chuẩn bị môi trường & Cognito](5.2-Prerequisite/)
3. [Triển khai Backend với AWS SAM](5.3-SAM-Backend/)
4. [Tích hợp WebSocket real-time](5.4-WebSocket-Realtime/)
5. [Lưu trữ Frontend tĩnh (S3 & CloudFront)](5.5-Frontend-Hosting/)
6. [Tự động hóa CI/CD với GitHub Actions](5.6-CICD-Pipeline/)
7. [Kiểm thử toàn trình](5.7-Integration-Test/)
8. [Dọn dẹp tài nguyên](5.8-Cleanup/)