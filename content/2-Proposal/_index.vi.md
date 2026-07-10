---
title: "Bản đề xuất"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# Cloud Battleship Arena
## Nền tảng Game Bắn Tàu Thời Gian Thực Đa Người Chơi trên AWS Serverless

### 1. Tóm tắt điều hành
**Cloud Battleship Arena** là phiên bản hiện đại, hỗ trợ nhiều người chơi theo thời gian thực của trò chơi Battleship. Dự án sử dụng kiến trúc **Serverless** trên **Amazon Web Services (AWS)** nhằm giảm công việc vận hành máy chủ, mở rộng theo nhu cầu và hỗ trợ giao tiếp real-time, kết hợp với giao diện phản hồi nhanh được xây dựng bằng **React 19 + Vite**.

Các thành phần Backend sử dụng dịch vụ được quản lý của AWS nên không cần vận hành máy chủ riêng. Tài nguyên được tính phí theo mức sử dụng và có thể tự mở rộng trong giới hạn dịch vụ được cấu hình.

---

### 2. Tuyên bố vấn đề

**Vấn đề hiện tại**

Ứng dụng nhiều người chơi cần duy trì trạng thái phòng, xử lý lượt chơi và đồng bộ dữ liệu giữa các Client. Việc tự vận hành Backend lâu dài có thể làm tăng công việc triển khai, giám sát và mở rộng. Với Battleship, hệ thống cũng cần phản hồi đủ nhanh để hai người chơi theo dõi cùng một trạng thái trận đấu.

**Giải pháp**

Cloud Battleship Arena tiếp cận vấn đề bằng kiến trúc **Serverless event-driven** trên AWS:

- **Amazon Cognito** xử lý đăng ký, đăng nhập và phiên người dùng; JWT Authorizer hiện bảo vệ các endpoint hồ sơ người dùng.
- **Amazon API Gateway (HTTP & WebSocket APIs)** cung cấp endpoint RESTful cho quản lý phòng/matchmaking và kênh WebSocket hai chiều cho game real-time.
- **AWS Lambda (Node.js 24.x)** thực thi toàn bộ logic backend: quản lý kết nối, định tuyến tin nhắn, ghép trận, xử lý lịch sử đấu.
- **Amazon DynamoDB** lưu trữ dữ liệu tại các bảng `Rooms`, `Connections`, `ChatMessages`, `User`, `EmailIndex` và `MatchHistoryV2`.
- **Amazon S3** lưu trữ Avatar người chơi thông qua Pre-signed URL.
- **Amazon CloudFront** phân phối Frontend tĩnh và nội dung Avatar qua CDN. Frontend gọi trực tiếp HTTP API và WebSocket API bằng endpoint của API Gateway.
- **AWS WAF** được gắn với CloudFront để giới hạn request bất thường đến lớp Frontend; Web ACL này không bảo vệ trực tiếp các endpoint API Gateway hiện tại.
- **AWS SAM (Serverless Application Model)** định nghĩa và triển khai Backend. Frontend hosting và monitoring dùng CloudFormation template riêng, còn Cognito được cấu hình thủ công.

**Lợi ích dự kiến**

- **Chi phí phù hợp môi trường demo**: Phần lớn Backend sử dụng mô hình pay-per-use; AWS WAF vẫn có phí cơ sở khi không có traffic.
- **Khả năng mở rộng theo nhu cầu**: Lambda, API Gateway và DynamoDB On-demand tự mở rộng trong quota và cấu hình của tài khoản.
- **Trải nghiệm thực tế với AWS**: Dự án bao gồm thiết kế kiến trúc, phát triển Backend/Frontend, triển khai, monitoring và pipeline tự động deploy Frontend.

---

### 3. Kiến trúc giải pháp

Ứng dụng sử dụng kiến trúc **Serverless** và hướng sự kiện (Event-driven) gồm 3 lớp chính:

1. **Frontend (SPA)**: React 19 + Vite được host tĩnh trên S3 và phân phối qua CloudFront, giao tiếp với Backend qua HTTP API và WebSocket.
2. **Real-time Engine**: Trong quá trình chơi game, thiết bị người chơi duy trì kết nối liên tục qua **WebSocket API** để đồng bộ trạng thái trò chơi (bắn đạn, xếp tàu, chat, kết thúc trận).
3. **Backend Serverless**: Các hàm **AWS Lambda** xử lý tính toán, truy cập an toàn vào **DynamoDB** để lưu trạng thái và thực thi logic game — không cần quản lý bất kỳ máy chủ nào.

#### Sơ đồ kiến trúc hệ thống

![Kiến trúc tổng quan hệ thống Cloud Battleship Arena](/images/5-Workshop/5.2-Prerequisite/AWS_Architecture.png)


#### Dịch vụ AWS sử dụng

| Dịch vụ | Vai trò |
|---|---|
| **Amazon Cognito** | Xác thực người dùng, quản lý JWT session token |
| **Amazon API Gateway (HTTP)** | Cung cấp RESTful endpoints: tạo phòng, ghép trận, hồ sơ |
| **Amazon API Gateway (WebSocket)** | Kênh giao tiếp hai chiều real-time trong suốt trận đấu |
| **AWS Lambda (Node.js 24.x)** | Toàn bộ logic game: connect/disconnect, fire, matchmaking, lịch sử |
| **Amazon DynamoDB** | Lưu trữ `Rooms`, `Connections`, `ChatMessages`, `User`, `EmailIndex`, `MatchHistoryV2`; TTL áp dụng cho dữ liệu phòng, kết nối và tin nhắn tạm thời |
| **Amazon S3** | Lưu trữ Avatar qua Pre-signed URL; host frontend tĩnh |
| **Amazon CloudFront** | Phân phối Frontend và Avatar qua OAC; không proxy HTTP/WebSocket API trong kiến trúc hiện tại |
| **AWS WAF** | Rate limiting tại lớp CloudFront; không bảo vệ trực tiếp API Gateway |
| **AWS SAM** | Infrastructure as Code cho Backend serverless |
| **GitHub Actions + OIDC** | Pipeline build và deploy Frontend lên S3/CloudFront |

#### Thiết kế thành phần

- **Frontend (React 19 + Vite)**: SPA với AWS Amplify cho auth, WebSocket Native cho real-time, Howler.js cho âm thanh, Phaser cho hiệu ứng động, hỗ trợ đa ngôn ngữ (i18n) và responsive design.
- **WebSocket Handlers**: `wsConnect`, `wsDisconnect`, `wsMessage` — quản lý kết nối, kiểm tra lượt chơi, tính hit/miss ở Backend và broadcast trạng thái game.
- **Room Management**: `createRoom`, `joinRoom`, `matchmakeRoom`, `getRoom`, `readyRoom`, `lobbyReadyRoom`, `rematchRoom`, `leaveRoom`.
- **User Management**: `createUser`, `getUser`, `updateUsername`, `getAvatarUploadUrl`, `getMatchHistory`.
- **Infra Layer**: CloudFormation stack riêng cho Frontend hosting (`frontend-hosting.yaml`) và CloudFront monitoring (`cloudfront-monitoring.yaml`).

---

### 4. Triển khai kỹ thuật

#### Các giai đoạn triển khai

Dự án được triển khai theo lộ trình thực tập 9 tuần:

1. **Đào tạo LAB & Kiến thức nền tảng** *(Tuần 1–4)*:
   - **Tuần 1**: Làm quen với AWS và các dịch vụ cơ bản trong AWS.
   - **Tuần 2**: Tìm hiểu Amazon S3, CloudFront và cách phân phối Frontend tĩnh bằng bucket private kết hợp OAC.
   - **Tuần 3**: Tìm hiểu IAM, Lambda, API Gateway và DynamoDB ở mức nền tảng.
   - **Tuần 4**: Xác định yêu cầu, phạm vi và kiến trúc ban đầu cho dự án Cloud Battleship Arena.

2. **Khởi động dự án & Phát triển Core Backend** *(Tuần 5–6)*:
   - **Tuần 5**: Tìm hiểu IAM User/Policy/Role, thảo luận đặc tả dự án Battleship và khởi tạo Frontend.
   - **Tuần 6**: Tích hợp AWS Cognito xác thực người dùng, thiết kế API Gateway & Lambda, lưu trữ DynamoDB và lên kế hoạch PvP WebSocket.

3. **Phát triển PvP, Game Logic & Tính năng nâng cao** *(Tuần 7–8)*:
   - **Tuần 7**: Hoàn thiện chơi PvP thời gian thực bằng WebSocket, tích hợp S3 Presigned URL tải avatar, hoàn thiện Profile và hiệu ứng âm thanh.
   - **Tuần 8**: Hoàn thiện matchmaking, chat, rematch, lịch sử trận đấu, leaderboard và hệ thống rank.

4. **Tối ưu hóa, Triển khai & Hoàn thiện tài liệu** *(Tuần 9)*:
   - **Tuần 9**: Cải tiến giao diện người dùng, Deploy Frontend bằng S3 & CloudFront, khắc phục lỗi Lambda Cold Start và vẽ sơ đồ kiến trúc AWS.

#### Yêu cầu kỹ thuật

- **Backend**: Node.js 24.x, AWS SAM CLI, AWS CLI.
- **Frontend**: Node.js v20+, React 19, Vite, AWS Amplify SDK, WebSocket Native, Howler.js, Phaser (hiệu ứng động).
- **Hạ tầng**: AWS Account và GitHub repository có Actions. Cognito được tạo/liên kết thủ công; room API và WebSocket cần được bổ sung JWT authorization trước khi dùng ở production.

---

### 5. Lộ trình & Mốc triển khai

Dự án phân bổ theo 4 giai đoạn tương ứng với 9 tuần thực tập (chi tiết tại Mục 4). Sau khi go-live, hệ thống được vận hành liên tục qua **CloudWatch Dashboard** để giám sát các chỉ số quan trọng:

| Giai đoạn | Tuần | Mốc hoàn thành |
|---|---|---|
| Kiến thức nền tảng | 1–4 | Làm quen với các dịch vụ AWS liên quan trực tiếp đến dự án và hoàn thành thiết kế ban đầu |
| Core Backend | 5–6 | Cognito, API Gateway, Lambda, DynamoDB hoạt động |
| PvP & Game Logic | 7–8 | WebSocket thời gian thực, Avatar, matchmaking, lịch sử trận đấu và rank |
| Triển khai & Hoàn thiện | 9 | Pipeline deploy Frontend, CloudFront và sơ đồ kiến trúc |
| **Vận hành liên tục** | Post go-live | CloudWatch monitoring, Cost optimization |

---

### 6. Ước tính ngân sách

Mô hình **Pay-per-use** giúp chi phí thấp khi lưu lượng ít, ngoại trừ phí cơ sở của AWS WAF. Bảng dưới đây là ước tính cho môi trường demo có traffic rất thấp; chi phí thực tế phụ thuộc Region, số request, dung lượng lưu trữ và chương trình Free Tier áp dụng cho tài khoản:

| Dịch vụ | Ước tính/tháng |
|---|---|
| AWS Lambda | ~$0.00 (Free Tier: 1M requests/tháng) |
| Amazon API Gateway (HTTP) | ~$0.01 (2,000 requests) |
| Amazon API Gateway (WebSocket) | ~$0.02 (connection minutes) |
| Amazon DynamoDB (On-demand) | ~$0.00 (Free Tier: 25 GB) |
| Amazon S3 (Avatar + Frontend) | ~$0.05 |
| Amazon CloudFront | ~$0.01 (Free Tier: 1 TB/tháng) |
| Amazon Cognito | ~$0.00 (Free Tier: 50,000 MAU) |
| AWS WAF | ~$6.00 (Không thuộc Free Tier: $5/Web ACL + $1/Rule) |
| **Tổng** | **~$6.10–$6.20/tháng** |

> *Lưu ý: AWS WAF có chi phí cơ sở khoảng $6.00/tháng ($5 cho mỗi Web ACL và $1 cho mỗi Rule), chưa bao gồm phí theo số request. Quyền lợi AWS Free Tier khác nhau theo thời điểm tạo tài khoản; cần kiểm tra AWS Pricing và Billing trước khi triển khai.*

---

### 7. Đánh giá rủi ro

#### Ma trận rủi ro

| Rủi ro | Mức ảnh hưởng | Xác suất | Giải pháp |
|---|---|---|---|
| Độ trễ WebSocket cao | Cao | Thấp | Chọn Region gần người dùng; tối ưu Lambda cold start |
| DynamoDB throttling | Trung bình | Thấp | On-demand billing tự scale; CloudWatch alarm |
| Chi phí vượt ngưỡng | Thấp | Thấp | Theo dõi AWS Billing, cấu hình Budget Alert khi vận hành dài hạn; TTL dọn dữ liệu tạm ở các bảng có hỗ trợ |
| Lỗi đồng bộ game state | Cao | Trung bình | Kiến trúc authoritative backend; validate phía server |
| Truy cập trái phép vào game API | Cao | Trung bình | JWT đã bảo vệ API người dùng; bổ sung authorization cho room API và WebSocket |
| Mất kết nối WebSocket | Trung bình | Trung bình | `wsDisconnect` dọn connection record; phát triển reconnect/recovery tự động phía Client |
| Thiếu kiểm thử tự động | Cao | Trung bình | Bổ sung unit/integration tests và chạy lint/test trong pipeline trước khi production |

#### Kế hoạch dự phòng

- Nếu mất kết nối WebSocket đột ngột, Backend dọn connection record qua `wsDisconnect`. Phiên bản hiện tại chưa tự reconnect/re-link vào phòng; đây là hạng mục cải tiến tiếp theo.
- Lưu mã nguồn và cấu hình IaC trong Git để có thể redeploy phiên bản đã kiểm tra khi triển khai gặp lỗi.
- CloudWatch Alarms gửi email khi Lambda errors hoặc DynamoDB throttle vượt ngưỡng.

---

### 8. Kết quả kỳ vọng

**Cải tiến kỹ thuật**
- Trải nghiệm game real-time ổn định cho hai người chơi, với trạng thái trận đấu được Backend xác thực và đồng bộ qua WebSocket.
- Khả năng mở rộng theo nhu cầu nhờ Lambda, API Gateway và DynamoDB On-demand, trong phạm vi quota dịch vụ.
- Pipeline Frontend: thay đổi trong `FrontEnd/` được push lên `main` hoặc `develop` sẽ tự động build, deploy lên S3 và invalidate CloudFront. Backend vẫn được deploy bằng SAM CLI.

**Giá trị học thuật & nghề nghiệp**
- Hiểu các nguyên tắc cơ bản của kiến trúc Serverless event-driven trên AWS thông qua dự án thực tế.
- Thực hành các giai đoạn Design → Code → Deploy → Monitor; automated testing là hạng mục cần tiếp tục hoàn thiện.
- Có kinh nghiệm thực hành với Cognito, API Gateway (HTTP + WebSocket), Lambda, DynamoDB, S3, CloudFront, SAM và CloudWatch.
