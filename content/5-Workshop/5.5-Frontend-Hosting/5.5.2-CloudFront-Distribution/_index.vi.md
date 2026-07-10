---
title : "Thiết lập CloudFront Distribution"
date : 2024-01-01
weight : 2
chapter : false
pre : " <b> 5.5.2 </b> "
---

### Mục tiêu

Triển khai toàn bộ hạ tầng Frontend (S3 Bucket, OAC, CloudFront Distribution) thông qua tệp CloudFormation `frontend-hosting.yaml` và xác nhận giao diện game chạy thành công trên HTTPS.

---

#### Custom Error Responses cho SPA Routing

Dự án React 19 dùng cơ chế Client-side Routing. Khi người chơi truy cập trực tiếp hoặc F5 tại `/room/ABC`, S3 trả về lỗi `403/404` vì thư mục này không tồn tại vật lý. CloudFront được cấu hình chuyển hướng toàn bộ lỗi về `index.html` với mã `200 OK` để React Router xử lý tiếp.

---

#### Triển khai Stack (AWS CloudFormation Console)

1. Mở **AWS Console → CloudFormation → Stacks → Create stack (with new resources)**.
2. **Prepare template**: Chọn **Template is ready**.
3. **Specify template**: Chọn **Upload a template file** → chọn tệp `/Infra/frontend-hosting.yaml`. Nhấn **Next**.
4. **Specify stack details**:
   - **Stack name**: `cloud-battleship-frontend-hosting-dev`.
   - **GitHubOwner**: `HaoAboutMe`.
   - **GitHubRepo**: `AWS_Cloud_Battleship_Arena`.
   - Nhấn **Next**.
5. **Configure stack options**: Giữ nguyên mặc định. Nhấn **Next**.
6. **Review**: Tích chọn **I acknowledge that AWS CloudFormation might create IAM resources** → Click **Submit**.

Quá trình deploy hoàn tất trong khoảng 3–5 phút.

---

#### Kiểm tra kết quả

Sau khi stack chuyển sang trạng thái `CREATE_COMPLETE`:

1. Chọn stack **cloud-battleship-frontend-hosting-dev** → Tab **Outputs**.
2. Lưu lại giá trị **CloudFrontDomainName** (ví dụ: `d12bu86qwnw1gk.cloudfront.net`).
3. Mở trình duyệt truy cập vào domain CloudFront và xác nhận giao diện game hiển thị trên HTTPS.

![CloudFront Outputs](/images/5-Workshop/5.5-Frontend-Hosting/5.5.2-CloudFront-Distribution/cloudfront-domain-output.png)

---

{{% notice warning %}}
**Hành động bắt buộc — Cập nhật CORS cho Backend**: Bạn đã có CloudFront domain. Hãy quay lại thư mục `BackEnd/` và chạy lại SAM deploy với domain vừa lấy:
```bash
cd AWS_Cloud_Battleship_Arena/BackEnd
sam deploy --parameter-overrides CorsOrigin=https://<YOUR_CLOUDFRONT_DOMAIN>
```
Không thực hiện bước này sẽ khiến API trả về lỗi CORS khi Frontend chạy từ CloudFront.
{{% /notice %}}
