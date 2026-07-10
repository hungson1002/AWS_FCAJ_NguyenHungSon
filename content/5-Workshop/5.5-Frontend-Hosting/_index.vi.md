---
title: "Lưu trữ Frontend tĩnh (S3 & CloudFront)"
date: 2024-01-01
weight: 5
chapter: false
pre: " <b> 5.5. </b> "
---

### Mục tiêu

Triển khai hạ tầng phân phối giao diện Frontend cho game **Cloud Battleship Arena** sử dụng mô hình tối ưu trên AWS: **Amazon S3** làm máy chủ lưu trữ tệp tĩnh (Hosting) phối hợp với **Amazon CloudFront** làm mạng lưới phân phối toàn cầu (CDN) và tăng cường bảo mật thông qua **Origin Access Control (OAC)**.

---

#### Tại sao dùng kết hợp S3 + CloudFront + OAC?

1. **Bảo mật tuyệt đối**: S3 Bucket được khóa hoàn toàn quyền truy cập công khai (Block Public Access). Người dùng chỉ có thể lấy file thông qua CloudFront CDN.
2. **Hỗ trợ Single Page Application (SPA)**: Cấu hình Error Responses để tự động chuyển hướng các lỗi 403/404 về `index.html` nhằm hỗ trợ điều hướng route phía client (React Router).
3. **Tối ưu tốc độ**: Nội dung tĩnh (JS, CSS, Ảnh) được lưu đệm (cache) tại các Edge Location gần người dùng nhất.

---

#### Nội dung bài học

- [Khởi tạo S3 Bucket & Cấu hình OAC](5.5.1-S3-Bucket/)
- [Thiết lập CloudFront Distribution](5.5.2-CloudFront-Distribution/)
- [Giám sát CDN CloudFront](5.5.3-CloudFront-Monitoring/)
- [Thiết lập Lưu trữ Avatar bảo mật (S3 & CloudFront OAC)](5.5.4-S3-Avatar-Bucket/)
- [Tích hợp AWS WAF bảo mật biên](5.5.5-WAF-Integration/)

