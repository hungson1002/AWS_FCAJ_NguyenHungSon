---
title : "Triển khai Backend với AWS SAM"
date : 2024-01-01 
weight : 3
chapter : false
pre : " <b> 5.3. </b> "
---

### Mục tiêu

Triển khai thành công toàn bộ Backend của Cloud Battleship Arena lên AWS — bao gồm **8 Lambda functions**, **2 API Gateways** (HTTP + WebSocket) và **3 DynamoDB tables** — sử dụng mẫu cấu hình `template.yaml`.

---

#### Bước 1: Build mã nguồn

```bash
cd BackEnd
sam build
```

---

#### Bước 2: Deploy lên AWS

```bash
sam deploy --guided
```

Nhập các thông số cấu hình khi được hỏi:

| Thông số | Giá trị |
|---|---|
| Stack Name | `cloud-battleship-backend-dev` |
| AWS Region | `ap-southeast-1` |
| CognitoUserPoolId | `ap-southeast-1_VV7CeCaWL` |
| CognitoUserPoolClientId | `1vmsoep8dq5nlr1qvf8hpgsvs2` |
| CorsOrigin | `http://localhost:5173` |

Sau khoảng 3–5 phút, quá trình triển khai hoàn tất.

---

#### Bước 3: Ghi lại thông tin Output

Lưu lại các giá trị URL từ phần **Outputs** của SAM:

```
HttpApiUrl    → https://elh9fh33rd.execute-api.ap-southeast-1.amazonaws.com
WebSocketUrl  → wss://b9mxr6sqg6.execute-api.ap-southeast-1.amazonaws.com/prod
```

---

#### Kiểm tra kết quả (AWS Console)

**CloudFormation Stack:**
1. Mở **AWS Console → CloudFormation → Stacks**.
2. Xác nhận stack `cloud-battleship-backend-dev` đang ở trạng thái `CREATE_COMPLETE`.

![CloudFormation stack CREATE_COMPLETE](/images/5-Workshop/5.3-SAM-Backend/cloudformation-stack.png)

**DynamoDB Tables:**
1. Mở **AWS Console → DynamoDB → Tables**.
2. Xác nhận 3 bảng `Rooms`, `Connections`, và `User` đã được khởi tạo thành công.

![DynamoDB 3 tables](/images/5-Workshop/5.3-SAM-Backend/dynamodb-tables.png)

---

#### Nội dung bài học

- [Cấu hình và Build SAM Stack](5.3.1-SAM-Build/)