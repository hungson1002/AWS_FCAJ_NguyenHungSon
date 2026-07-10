---
title : "Tích hợp AWS WAF bảo mật biên"
date : 2024-01-01
weight : 4
chapter : false
pre : " <b> 5.5.4 </b> "
---

### Mục tiêu

Bảo vệ giao diện frontend tĩnh của game và ngăn chặn các cuộc tấn công DDoS/Brute-force cơ bản bằng cách đính kèm **AWS WAF (Web Application Firewall)** vào **CloudFront Distribution** của Frontend.

---

### Các bước thiết lập AWS WAF

#### 1. Tạo Web ACL (Access Control List) mới
1. Truy cập **AWS WAF Console** > Menu bên trái chọn **Web ACLs**.
2. Ở phần **Region**, chọn **Global (CloudFront)** (Bắt buộc phải chọn Global vì CloudFront là dịch vụ biên toàn cầu).
3. Nhấp **Create web ACL**.
4. Cấu hình thông tin cơ bản:
   - **Name**: `CloudBattleship-WAF`.
5. Bỏ qua bước liên kết tài nguyên (Associated AWS resources) trong wizard khởi tạo ban đầu, nhấp **Next** qua các cấu hình Rule, Metrics và nhấp **Create web ACL** để hoàn thành tạo WAF trước.

---

#### 2. Gắn Web ACL vào CloudFront Distribution (Sau khi đã tạo xong WAF)
Sau khi Web ACL `CloudBattleship-WAF` đã được tạo thành công, tiến hành liên kết tài nguyên như sau:
1. Tại danh sách Web ACLs, nhấp vào tên Web ACL `CloudBattleship-WAF` vừa tạo.
2. Chọn tab **Associated AWS resources** > Nhấp nút **Add global resources**.
3. Từ menu thả xuống, chọn **Add CloudFront or Amplify resources**.

![Quản lý tài nguyên bảo vệ](/images/5-Workshop/5.5-Frontend-Hosting/5.5.4-WAF-Integration/manage_resource.png)

4. Trong cửa sổ **Select resources to protect**, tích chọn CloudFront Distribution của Frontend của bạn (ví dụ: `<YOUR_CLOUDFRONT_DISTRIBUTION_ID> - <YOUR_CLOUDFRONT_DOMAIN_NAME> - cloud-battleship-arena dev frontend`).

![Chọn CloudFront để bảo vệ](/images/5-Workshop/5.5-Frontend-Hosting/5.5.4-WAF-Integration/select_resource.png)

5. Nhấp **Add** để hoàn tất liên kết.

---

#### 3. Tạo Rule giới hạn tần suất gửi yêu cầu (Rate-based Rule)
Để chống spam request từ một địa chỉ IP duy nhất:
1. Trong giao diện chi tiết của Web ACL `CloudBattleship-WAF`, chọn tab **Rules** > click **Add rules** > Chọn **Add my own rules and rule groups**.
2. Trong bảng bên phải "Add new rule", tích chọn **Custom rule** và chọn **Next**.

![Chọn Custom rule](/images/5-Workshop/5.5-Frontend-Hosting/5.5.4-WAF-Integration/manage_rule.png)

3. Tại màn hình cấu hình chi tiết, chọn loại rule và thiết lập các thông tin sau:
   - **Rule type**: Chọn **Rate-based rule**.
   - **Rule name**: Đặt tên (ví dụ: `SpamLimitRule`).
   - **Rate limit**: Nhập `100` (Giới hạn tối đa 100 request trên mỗi địa chỉ IP trong vòng 5 phút).
   - **IP address to use for rate limiting**: Chọn **Source IP address**.
   - **Action**: Chọn **Block** để chặn các request vượt ngưỡng.

![Cấu hình Rate-based Rule](/images/5-Workshop/5.5-Frontend-Hosting/5.5.4-WAF-Integration/set_based_limit.png)

4. Nhấn **Add rule** ở cuối màn hình và lưu thay đổi.
