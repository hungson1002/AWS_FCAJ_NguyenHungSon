---
title : "Thiết lập Lưu trữ Avatar bảo mật (S3 & CloudFront OAC)"
date : 2024-01-01
weight : 3
chapter : false
pre : " <b> 5.5.3 </b> "
---

### Mục tiêu

Để người dùng tải và hiển thị ảnh đại diện (avatar) một cách an toàn, hệ thống sử dụng **Amazon S3** kết hợp **Amazon CloudFront** và cơ chế **Origin Access Control (OAC)**. 

S3 Avatar Bucket (ví dụ: `cloud-battleship-backend-dev-avatarbucket-vp9oxrrlmrsu` - *lưu ý: hậu tố `vp9oxrrlmrsu` là ngẫu nhiên và sẽ khác với bucket thực tế của bạn*) đã được tạo tự động thông qua tệp SAM template của Backend. Trong bước này, chúng ta sẽ cấu hình thêm S3 Avatar Bucket này làm một **Origin** mới trong **CloudFront Distribution** hiện tại (`BattleshipArena`) và thiết lập cơ chế Cache Invalidation bằng tay để đồng bộ hóa cập nhật hình ảnh.

---

### Các bước thiết lập

#### 1. Khởi tạo Origin Access Control (OAC) cho S3 Avatar
1. Truy cập **CloudFront Console** > Menu bên trái chọn **Origin access** > Nhấp **Create control setting**.
2. Đặt cấu hình:
   - **Name**: `battleship-arena-AvatarOAC`.
   - **Signing behavior**: Chọn **Sign requests (recommended)**.
   - **Origin type**: Chọn **S3**.
3. Nhấn **Create** để hoàn thành tạo OAC.

---

#### 2. Thêm S3 Avatar Origin vào CloudFront Distribution hiện tại
1. Truy cập **CloudFront Console** > Nhấp vào CloudFront Distribution của bạn (ví dụ: `BattleshipArena` hoặc mã distribution `E1QBTJG476LN8`).
2. Chọn tab **Origins** > Nhấp **Create origin**.
3. Cấu hình các thông tin origin như sau:
   - **Origin domain**: Chọn S3 Avatar Bucket đã được tạo từ template (chọn bucket có tên tương tự dạng `cloud-battleship-backend-dev-avatarbucket-YOUR_UNIQUE_SUFFIX.s3.ap-southeast-1.amazonaws.com`, trong đó `YOUR_UNIQUE_SUFFIX` là hậu tố ngẫu nhiên do SAM sinh ra).
   - **Origin access**: Tích chọn **Origin access control settings (recommended)**.
   - **Origin access control**: Chọn `battleship-arena-AvatarOAC` vừa tạo ở bước trên (hoặc nhấp **Create new OAC** nếu chưa tạo trước đó).

![Tạo Origin cho Avatar Bucket](/images/5-Workshop/5.5-Frontend-Hosting/5.5.3-S3-Avatar-Bucket/create_origin.jpg)

4. Nhấp **Create origin** ở cuối trang để hoàn tất thêm Origin.

---

#### 3. Tạo Cache Behavior cho đường dẫn Avatar

Việc thêm Origin chưa đủ — bạn cần tạo thêm **Cache Behavior** để CloudFront biết dùng Avatar Origin khi có request đến đường dẫn `/avatars/*`:
1. Trong CloudFront Distribution (`BattleshipArena`), chọn tab **Behaviors** > Nhấp **Create behavior**.
2. Cấu hình các thông tin sau:
   - **Path pattern**: `/avatars/*`
   - **Origin and origin groups**: Chọn Avatar S3 Origin vừa tạo ở bước trên (tên dạng `cloud-battleship-backend-dev-avatarbucket-...`).
   - **Viewer protocol policy**: Chọn **Redirect HTTP to HTTPS**.
   - **Cache policy**: Chọn **CachingOptimized** (mặc định).
3. Nhấp **Create behavior** để lưu.

> **Vì sao cần bước này?** Nếu không có Cache Behavior riêng cho `/avatars/*`, CloudFront sẽ dùng behavior mặc định (default `*`) để phục vụ các request avatar — nhưng behavior mặc định trỏ về S3 Frontend Bucket, không phải Avatar Bucket. Kết quả là avatar sẽ không load được.

---

#### 4. Cập nhật S3 Bucket Policy
Để CloudFront OAC có quyền đọc tệp từ S3 Bucket đã tạo, bạn cần cập nhật Bucket Policy trên S3:
1. Truy cập **Amazon S3 Console** > Chọn bucket của bạn (ví dụ: `cloud-battleship-backend-dev-avatarbucket-YOUR_UNIQUE_SUFFIX`).
2. Chọn tab **Permissions** > Kéo xuống phần **Bucket policy** và nhấn **Edit**.
3. Dán chính sách JSON sau (thay thế bằng Account ID, CloudFront Distribution ID và **tên S3 Avatar Bucket thực tế của bạn**):

```json
{
    "Version": "2008-10-17",
    "Statement": [
        {
            "Sid": "AllowCloudFrontServicePrincipalReadOnly",
            "Effect": "Allow",
            "Principal": {
                "Service": "cloudfront.amazonaws.com"
            },
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::YOUR_S3_AVATAR_BUCKET_NAME/*",
            "Condition": {
                "StringEquals": {
                    "AWS:SourceArn": "arn:aws:cloudfront::YOUR_ACCOUNT_ID:distribution/YOUR_CLOUDFRONT_DIST_ID"
                }
            }
        }
    ]
}
```
4. Nhấn **Save changes**.

---

#### 5. Cấu hình CloudFront Cache Invalidation bằng tay
Khi người chơi tải lên ảnh đại diện mới ghi đè lên ảnh cũ (cùng tên tệp tin ví dụ `avatars/userId.jpg`), CloudFront có thể vẫn trả về ảnh cũ do cơ chế Cache. Ta cần tạo một lệnh Invalidation bằng tay để xóa cache:
1. Truy cập **CloudFront Console** > Chọn Distribution của S3 Avatar (`BattleshipArena`).
2. Chọn tab **Invalidations** > Nhấp **Create invalidation**.
3. Trong ô **Object paths**, nhập đường dẫn chứa ảnh đại diện cần làm mới:
   - Nhập `/avatars/*` (để làm mới toàn bộ ảnh đại diện) hoặc chỉ định chính xác `/avatars/YOUR_USER_ID.jpg`.
4. Nhấp **Create invalidation** và chờ trạng thái hoàn thành để kiểm tra ảnh đại diện mới đã được cập nhật trực tiếp trên giao diện client.
