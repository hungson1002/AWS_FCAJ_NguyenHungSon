---
title: "Dọn dẹp tài nguyên"
date: 2026-07-10 
weight: 8
chapter: false
pre: " <b> 5.8. </b> "
---

### Mục tiêu

Xóa toàn bộ các tài nguyên Backend và Frontend đã tạo trên AWS để tránh phát sinh chi phí ngoài ý muốn.

{{% notice warning %}}
**Lưu ý chi phí**: AWS WAF phát sinh chi phí cố định **~$6.00/tháng** ($5/Web ACL + $1/Rule) ngay cả khi không có traffic. Hãy xóa WAF **trước tiên** trước các tài nguyên còn lại.
{{% /notice %}}

---

#### Bước 1: Xóa Backend Stack (SAM CLI)

```bash
cd BackEnd
sam delete --stack-name cloud-battleship-backend-dev
```

Nhập `y` khi được hỏi xác nhận.

---

#### Bước 2: Xóa Monitoring Stack (CloudFormation Console — us-east-1)

1. **Chuyển vùng** sang **us-east-1**.
2. Mở **CloudFormation → Stacks**.
3. Chọn stack **cloud-battleship-arena-monitoring** → Click **Delete** → Xác nhận.

---

#### Bước 3: Xóa Frontend Stack (CloudFormation Console)

1. Chuyển vùng về **ap-southeast-1**.
2. Mở **CloudFormation → Stacks**.
3. Chọn stack **cloud-battleship-frontend-hosting-dev** → Click **Delete** → Xác nhận.
4. Đợi trạng thái chuyển sang `DELETE_COMPLETE`.

---

#### Bước 4: Xóa AWS WAF Web ACL (WAF Console — Global)

{{% notice warning %}}
WAF Web ACL thuộc **Global (CloudFront)** — phải truy cập ở vùng **us-east-1** mới thấy.
{{% /notice %}}

1. **Chuyển vùng** sang **us-east-1 (N. Virginia)**.
2. Mở **AWS WAF & Shield Console → Web ACLs**.
3. Chọn Web ACL **CloudBattleship-WAF**.
4. Vào tab **Associated AWS resources** → Xác nhận không còn CloudFront Distribution nào được gắn (nếu còn, nhấn **Remove** để tách ra trước).
5. Quay lại danh sách **Web ACLs** → Chọn **CloudBattleship-WAF** → Click **Delete** → Xác nhận.

---

#### Bước 5: Xóa Cognito User Pool (Cognito Console)

1. Mở **AWS Console → Cognito → User pools**.
2. Chọn User Pool **Battleship-Arena**.
3. Click **Delete user pool** → Nhập tên User Pool để xác nhận → Click **Delete**.

---

#### Checklist dọn dẹp

- [ ] Stack `cloud-battleship-backend-dev` đã xóa thành công.
- [ ] Stack `cloud-battleship-frontend-hosting-dev` đã xóa thành công.
- [ ] Stack `cloud-battleship-arena-monitoring` đã xóa thành công.
- [ ] S3 Bucket đã được dọn sạch và xóa.
- [ ] CloudFront Distribution đã bị vô hiệu hóa.
- [ ] Web ACL `CloudBattleship-WAF` đã xóa hoàn toàn (kiểm tra tại us-east-1).
- [ ] Cognito User Pool `Battleship-Arena` đã xóa hoàn toàn.

---

#### Tổng kết

Chúc mừng bạn đã hoàn thành toàn bộ bài thực hành! Bạn đã làm quen và triển khai thành công các công nghệ Cloud thực chiến:

| Chủ đề | Công nghệ áp dụng |
|---|---|
| **Infrastructure as Code** | AWS SAM, AWS CloudFormation |
| **Serverless Compute** | AWS Lambda (Node.js 24.x) |
| **REST & Real-time API** | Amazon API Gateway (HTTP & WebSocket) |
| **NoSQL Database** | Amazon DynamoDB (TTL, GSI, On-demand) |
| **Static Hosting & CDN** | Amazon S3 + Amazon CloudFront (OAC) |
| **Automated CI/CD** | GitHub Actions + AWS OpenID Connect |
| **Cloud Security** | IAM Least Privilege, Cognito JWT Auth, AWS WAF |