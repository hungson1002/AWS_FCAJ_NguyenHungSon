---
title : "Khởi tạo S3 Bucket & Cấu hình OAC"
date : 2024-01-01
weight : 1
chapter : false
pre : " <b> 5.5.1 </b> "
---

### Mục tiêu

Hiểu cách tệp IaC `frontend-hosting.yaml` định nghĩa S3 Bucket bảo mật và cơ chế OAC cho phép CloudFront truy xuất nội dung mà không cần mở quyền công cộng.

---

#### Cách hoạt động của S3 + OAC

Trong dự án, toàn bộ hạ tầng Frontend được quản lý bằng tệp `/Infra/frontend-hosting.yaml`:

- **S3 Bucket**: Khai báo với `PublicAccessBlockConfiguration` bật đầy đủ — chặn mọi truy cập trực tiếp từ internet.
- **Origin Access Control (OAC)**: Ký header SigV4 trên mỗi request CloudFront gửi đến S3 làm bằng chứng định danh.
- **S3 Bucket Policy**: Chỉ cho phép duy nhất Distribution CloudFront của dự án có quyền `s3:GetObject`.

Tất cả 3 tài nguyên này được tạo tự động khi deploy stack CloudFormation ở bước 5.5.2.

---

#### Kiểm tra cấu hình sau khi deploy

{{% notice warning %}}
Bước này yêu cầu stack CloudFormation phải **đã tồn tại**. Hãy thực hiện **bước 5.5.2 (Deploy Stack)** trước, sau đó quay lại đây để xác nhận.
{{% /notice %}}

Sau khi stack `cloud-battleship-frontend-hosting-dev` được tạo thành công, xác nhận S3 Bucket chặn đúng:

```bash
aws s3api get-public-access-block \
  --bucket cloud-battleship-frontend-hosting-d-frontendbucket-41a7wzdtapv0 \
  --region ap-southeast-1
```

Kết quả trả về phải có cả 4 trường đều là `true`:
```json
{
    "PublicAccessBlockConfiguration": {
        "BlockPublicAcls": true,
        "IgnorePublicAcls": true,
        "BlockPublicPolicy": true,
        "RestrictPublicBuckets": true
    }
}
```

---

#### Xác nhận trong S3 Console

![S3 Block Public Access](/images/5-Workshop/5.5-Frontend-Hosting/5.5.1-S3-Bucket/public-access-block.png)

