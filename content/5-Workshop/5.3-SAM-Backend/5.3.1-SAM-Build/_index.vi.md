---
title : "Cấu hình và Build SAM Stack"
date : 2024-01-01 
weight : 1
chapter : false
pre : " <b> 5.3.1 </b> "
---

#### Hiểu cấu trúc template.yaml

File `template.yaml` là trái tim của Backend. Mở file và xem qua các phần chính:

```yaml
AWSTemplateFormatVersion: "2010-09-09"
Transform: AWS::Serverless-2016-10-31
Description: Cloud Battleship Arena serverless backend.

Globals:
  Function:
    Runtime: nodejs24.x
    Timeout: 10
    MemorySize: 256
```

**Globals** định nghĩa cấu hình mặc định cho tất cả Lambda function — giúp tránh lặp lại cấu hình ở mỗi function.

#### Bước 1: Cài đặt dependencies

Di chuyển vào thư mục `BackEnd/`:

```bash
cd AWS_Cloud_Battleship_Arena/BackEnd
npm install
```

#### Bước 2: Build SAM artifact

```bash
sam build
```

Quá trình build sẽ:
1. Đọc `template.yaml` và xác định tất cả hàm Lambda.
2. Chạy `npm install` cho mỗi hàm.
3. Đóng gói artifact vào thư mục `.aws-sam/build/`.

Sau khi build thành công, bạn sẽ thấy:

```
Build Succeeded

Built Artifacts  : .aws-sam/build
Built Template   : .aws-sam/build/template.yaml

Commands you can use next
=========================
[*] Validate SAM template: sam validate
[*] Invoke Function: sam local invoke
[*] Test Function in the Cloud: sam sync --stack-name {{stack-name}} --watch
[*] Deploy: sam deploy --guided
```

#### Bước 3: Validate template (tùy chọn)

```bash
sam validate --lint
```

Lệnh này kiểm tra cú pháp và logic của `template.yaml` để phát hiện lỗi trước khi deploy.

#### Bước 4: Xem trước changeset trước khi deploy

```bash
sam deploy --guided --no-execute-changeset
```

Lệnh này hiển thị những thay đổi nào sẽ được tạo mà không thực sự deploy. Hữu ích để review trước khi áp dụng.

{{% notice tip %}}
Sau lần đầu chạy `sam deploy --guided`, SAM tự lưu tham số vào file `samconfig.toml`. Các lần deploy tiếp theo chỉ cần chạy `sam deploy` — không cần `--guided`.
{{% /notice %}}
