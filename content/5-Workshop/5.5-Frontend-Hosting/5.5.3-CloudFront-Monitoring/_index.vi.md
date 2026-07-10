---
title : "Giám sát CDN CloudFront"
date : 2024-01-01
weight : 3
chapter : false
pre : " <b> 5.5.3 </b> "
---

### Mục tiêu

Triển khai stack giám sát CloudFront để cấu hình cảnh báo lỗi gửi qua Email (SNS Alarm) và tạo CloudWatch Dashboard theo dõi lưu lượng trực quan.

---

{{% notice warning %}}
Toàn bộ chỉ số (metrics) của Amazon CloudFront được AWS báo cáo về vùng **us-east-1 (N. Virginia)**. Stack giám sát này bắt buộc phải được deploy tại `us-east-1`.
{{% /notice %}}

---

#### Triển khai Stack giám sát (AWS CloudFormation Console)

1. **Chuyển vùng** trên AWS Console sang **us-east-1 (N. Virginia)**.
2. Mở **CloudFormation → Stacks → Create stack (with new resources)**.
3. Chọn **Upload a template file** → chọn `/Infra/cloudfront-monitoring.yaml`. Nhấn **Next**.
4. **Specify stack details**:
   - **Stack name**: `cloud-battleship-arena-monitoring`.
   - **CloudFrontDistributionId**: `<YOUR_CLOUDFRONT_DISTRIBUTION_ID>` (Nhập Distribution ID CloudFront của bạn đã copy ở bước 5.5.2).
   - **MonitoringAlarmEmail**: Nhập địa chỉ Email của bạn.
   - Nhấn **Next**.
5. Nhấn **Next** và click **Submit** để deploy.

---

#### Xác nhận đăng ký SNS Email

Sau khi stack tạo xong, AWS SNS gửi email xác nhận đến hòm thư của bạn:

1. Mở hộp thư email.
2. Tìm email từ **AWS Notifications** có tiêu đề `AWS Notification - Subscription Confirmation`.
3. Click **Confirm subscription**.

![Email xác nhận đăng ký SNS Alarm](/images/5-Workshop/5.5-Frontend-Hosting/5.5.3-CloudFront-Monitoring/sns-confirm.png)

---

#### Kiểm tra tài nguyên giám sát

**CloudWatch Alarms** (đảm bảo đang ở vùng `us-east-1`):

![Danh sách CloudWatch Alarms đã được tạo](/images/5-Workshop/5.5-Frontend-Hosting/5.5.3-CloudFront-Monitoring/cloudfront-alarms.png)

Mở **AWS Console → CloudWatch → Alarms**. Xác nhận 2 Alarm giám sát lỗi CloudFront đang hiển thị.

**CloudWatch Dashboard:**

![Giao diện CloudWatch Dashboard](/images/5-Workshop/5.5-Frontend-Hosting/5.5.3-CloudFront-Monitoring/cloudfront-dashboard.png)

Mở **CloudWatch → Dashboards** và chọn dashboard vừa tạo để xem biểu đồ requests và traffic.
