---
title : "Kiểm thử toàn trình"
date : 2026-07-10
weight : 7
chapter : false
pre : " <b> 5.7. </b> "
---

### Mục tiêu

Xác thực toàn bộ luồng hoạt động thực tế của game bằng cách tạo tài khoản qua Cognito, kết nối phòng đấu và tiến hành chơi game trực tiếp trên trình duyệt web.

{{% notice info %}}
**Trải nghiệm Demo trực tiếp:** Cloud Battleship Arena đã được triển khai hoàn chỉnh tại:
> [https://d12bu86qwnw1gk.cloudfront.net/](https://d12bu86qwnw1gk.cloudfront.net/)
{{% /notice %}}

---

#### Danh sách kiểm tra trước khi test (Pre-check)

{{% notice warning %}}
Trước khi mở trình duyệt, hãy xác nhận đủ các điều kiện sau. Nếu bỏ qua bất kỳ mục nào, game sẽ báo lỗi CORS hoặc không load được.
{{% /notice %}}

- [ ] Đã redeploy SAM với `CorsOrigin=https://<YOUR_CLOUDFRONT_DOMAIN>` tại bước 5.5.2 chưa?
- [ ] Frontend đã được build và upload lên S3 Bucket chưa? (qua CI/CD ở 5.6 hoặc upload thủ công `aws s3 sync dist/ s3://<BUCKET>/`)
- [ ] Truy cập CloudFront domain trên trình duyệt có hiển thị giao diện game không (không phải trang trắng)?

---

#### Kiểm thử qua giao diện Web (Web UI)

1. Mở trình duyệt và truy cập vào tên miền **CloudFrontDomainName** đã lấy từ Outputs ở bước 5.5.
2. Click **Đăng ký (Sign Up)** để tạo 2 tài khoản người chơi.
3. Mở **2 cửa sổ trình duyệt song song** để đăng nhập bằng 2 tài khoản vừa tạo.
4. Chọn chế độ ghép trận trực tuyến bằng cách:
   - Click **Play vs Player (Ranked)** -> chọn **Join Queue** ở cả hai tài khoản để hệ thống tự động tìm đối thủ và kết nối vào phòng.
   - *(Hoặc có thể chọn Private room để tạo phòng và mời bằng code phòng).*
5. Sắp xếp vị trí các tàu chiến trên bản đồ 10x10 (có thể click **Auto Arrange**) và click **Sẵn sàng (Ready)** ở cả hai trình duyệt để bắt đầu trận đấu.
6. Thực hiện lượt bắn đạn qua lại trên bản đồ. Xác nhận hiệu ứng trúng/trượt và lượt chơi được đồng bộ tức thì qua WebSocket.
7. Chơi đến khi một bên chiến thắng để hệ thống cập nhật điểm Rank ELO.

**Các bước ghép trận và chuẩn bị chiến đấu:**

![Giao diện chọn chế độ chơi](/images/5-Workshop/5.7-Integration-Test/gameplay-mode-select.png)

![Chọn chế độ chơi Ranked Battle](/images/5-Workshop/5.7-Integration-Test/gameplay-battle-protocol.png)

![Hệ thống đang tìm đối thủ](/images/5-Workshop/5.7-Integration-Test/gameplay-matchmaking-queue.png)

![Đã kết nối đối thủ và sẵn sàng vào trận](/images/5-Workshop/5.7-Integration-Test/gameplay-opponent-found.png)

![Sắp đặt đội hình tàu chiến](/images/5-Workshop/5.7-Integration-Test/gameplay-fleet-staging.png)

![Giao diện chiến đấu thời gian thực](/images/5-Workshop/5.7-Integration-Test/gameplay-fighting.png)

---

#### Kiểm tra kết quả (AWS Console)

**Kết quả trận đấu trên Web UI:**

![Kết quả chiến thắng trận đấu](/images/5-Workshop/5.7-Integration-Test/gameplay-victory.png)

**DynamoDB User Table:**

1. Mở **AWS Console → DynamoDB → Tables → User**.
2. Chọn **Explore table items**.
3. Xác nhận điểm Rank (ELO) của người chơi thắng cuộc đã được cộng điểm thành công trong bảng cơ sở dữ liệu.

![Xác nhận điểm ELO được cập nhật trong DynamoDB](/images/5-Workshop/5.7-Integration-Test/dynamodb-elo-update.png)
