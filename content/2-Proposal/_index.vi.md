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
**Cloud Battleship Arena** là một phiên bản hiện đại, hỗ trợ nhiều người chơi theo thời gian thực (Real-time Multiplayer) của tựa game bắn tàu kinh điển (Battleship). Dự án sử dụng kiến trúc **Serverless** hoàn toàn trên **Amazon Web Services (AWS)** để đảm bảo khả năng mở rộng cực cao và độ trễ thấp nhất, kết hợp với giao diện người dùng sống động, phản hồi nhanh được xây dựng bằng **React 19 + Vite**.

Toàn bộ hệ thống không cần quản lý bất kỳ máy chủ vật lý nào — AWS tự động mở rộng theo lượng người chơi, giúp tối ưu chi phí và tăng độ tin cậy cho trải nghiệm game thời gian thực.

---

### 2. Tuyên bố vấn đề

**Vấn đề hiện tại**

Các trò chơi nhiều người chơi truyền thống thường yêu cầu máy chủ vật lý cố định, dẫn đến chi phí vận hành cao, khó mở rộng theo lượng người dùng và phức tạp trong việc bảo trì. Đặc biệt với game real-time như Battleship, độ trễ mạng là yếu tố cực kỳ quan trọng — mỗi mili-giây đều ảnh hưởng đến trải nghiệm người chơi.

**Giải pháp**

Cloud Battleship Arena giải quyết vấn đề này bằng kiến trúc **Serverless event-driven** hoàn toàn trên AWS:

- **Amazon Cognito** xử lý xác thực người dùng an toàn.
- **Amazon API Gateway (HTTP & WebSocket APIs)** cung cấp endpoint RESTful cho quản lý phòng/matchmaking và kênh WebSocket hai chiều cho game real-time.
- **AWS Lambda (Node.js 24.x)** thực thi toàn bộ logic backend: quản lý kết nối, định tuyến tin nhắn, ghép trận, xử lý lịch sử đấu.
- **Amazon DynamoDB** lưu trữ dữ liệu tốc độ cao (siêu thấp độ trễ) cho các bảng `Rooms`, `Connections`, `ChatMessages`, `User`, `MatchHistory`.
- **Amazon S3** lưu trữ Avatar người chơi thông qua Pre-signed URL.
- **Amazon CloudFront** phân phối Frontend tĩnh toàn cầu với độ trễ thấp.
- **AWS SAM (Serverless Application Model)** định nghĩa, đóng gói và triển khai toàn bộ hạ tầng dưới dạng mã (IaC).

**Lợi ích và hoàn vốn đầu tư (ROI)**

- **Chi phí gần bằng 0 khi không có người chơi**: Mô hình pay-per-use của AWS chỉ tính phí khi có request thực tế.
- **Khả năng mở rộng tự động**: Không cần lo lắng về traffic đột biến.
- **Trải nghiệm thực tế với AWS**: Dự án bao gồm đầy đủ vòng đời phát triển — từ thiết kế kiến trúc, lập trình Backend/Frontend, đến CI/CD tự động với GitHub Actions.

---

### 3. Kiến trúc giải pháp

Ứng dụng tuân theo hoàn toàn kiến trúc **Serverless** và hướng sự kiện (Event-driven) gồm 3 lớp chính:

1. **Frontend (SPA)**: React 19 + Vite được host tĩnh trên S3 và phân phối qua CloudFront, giao tiếp với Backend qua HTTP API và WebSocket.
2. **Real-time Engine**: Trong quá trình chơi game, thiết bị người chơi duy trì kết nối liên tục qua **WebSocket API** để đồng bộ trạng thái trò chơi (bắn đạn, xếp tàu, chat, kết thúc trận).
3. **Backend Serverless**: Các hàm **AWS Lambda** xử lý tính toán, truy cập an toàn vào **DynamoDB** để lưu trạng thái và thực thi logic game — không cần quản lý bất kỳ máy chủ nào.

#### Dịch vụ AWS sử dụng

| Dịch vụ | Vai trò |
|---|---|
| **Amazon Cognito** | Xác thực người dùng, quản lý JWT session token |
| **Amazon API Gateway (HTTP)** | Cung cấp RESTful endpoints: tạo phòng, ghép trận, hồ sơ |
| **Amazon API Gateway (WebSocket)** | Kênh giao tiếp hai chiều real-time trong suốt trận đấu |
| **AWS Lambda (Node.js 24.x)** | Toàn bộ logic game: connect/disconnect, fire, matchmaking, lịch sử |
| **Amazon DynamoDB** | Lưu trữ `Rooms`, `Connections`, `ChatMessages`, `User`, `MatchHistory` với TTL tự động |
| **Amazon S3** | Lưu trữ Avatar qua Pre-signed URL; host frontend tĩnh |
| **Amazon CloudFront** | Phân phối Frontend toàn cầu, HTTPS, cache tối ưu |
| **AWS SAM** | Infrastructure as Code — định nghĩa và triển khai hạ tầng |
| **GitHub Actions + OIDC** | CI/CD tự động: build và deploy Frontend lên S3/CloudFront |

#### Thiết kế thành phần

- **Frontend (React 19 + Vite)**: SPA với AWS Amplify cho auth, WebSocket Native cho real-time, Howler.js cho âm thanh, Phaser cho hiệu ứng động, hỗ trợ đa ngôn ngữ (i18n) và responsive design.
- **WebSocket Handlers**: `wsConnect`, `wsDisconnect`, `wsMessage` — quản lý vòng đời kết nối và phát sóng (broadcast) trạng thái game.
- **Room Management**: `createRoom`, `joinRoom`, `matchmakeRoom`, `getRoom`, `readyRoom`, `lobbyReadyRoom`, `rematchRoom`, `leaveRoom`.
- **User Management**: `createUser`, `getUser`, `updateUsername`, `getAvatarUploadUrl`, `getMatchHistory`.
- **Infra Layer**: CloudFormation stack riêng cho Frontend hosting (`frontend-hosting.yaml`) và CloudFront monitoring (`cloudfront-monitoring.yaml`).

---

### 4. Triển khai kỹ thuật

#### Các giai đoạn triển khai

Dự án được chia làm 3 giai đoạn lớn:

1. **Nghiên cứu & Thiết kế kiến trúc** *(Trước thực tập)*:
   - Nghiên cứu kiến trúc Serverless trên AWS.
   - Thiết kế luồng WebSocket cho game real-time.
   - Xác định các bảng DynamoDB và lược đồ dữ liệu.

2. **Phát triển Backend & Frontend** *(Tháng 1–2)*:
   - Lập trình các hàm Lambda cho WebSocket, Room, User Management.
   - Định nghĩa toàn bộ hạ tầng bằng AWS SAM (`template.yaml`).
   - Xây dựng giao diện React 19 với đầy đủ tính năng game.
   - Tích hợp AWS Amplify cho luồng xác thực Cognito.

3. **Kiểm thử, CI/CD & Triển khai** *(Tháng 2–3)*:
   - Thiết lập GitHub Actions với OIDC để tự động deploy Frontend.
   - Cấu hình CloudFront, S3 Bucket Policy, CloudWatch Alarms.
   - Kiểm thử end-to-end: luồng WebSocket, matchmaking, lịch sử đấu.
   - Đưa vào vận hành thực tế.

#### Yêu cầu kỹ thuật

- **Backend**: Node.js 24.x, AWS SAM CLI, AWS CLI.
- **Frontend**: Node.js v20+, React 19, Vite, AWS Amplify SDK, WebSocket Native, Howler.js, Phaser (hiệu ứng động).
- **Hạ tầng**: AWS Account, GitHub repository với Actions enabled.

---

### 5. Lộ trình & Mốc triển khai

| Giai đoạn | Thời gian | Nội dung |
|---|---|---|
| Trước thực tập | Tháng 0 | Nghiên cứu Serverless, thiết kế kiến trúc, vẽ luồng WebSocket |
| Tháng 1 | Tuần 1–4 | Phát triển Backend Lambda + SAM, kết nối DynamoDB |
| Tháng 2 | Tuần 5–8 | Phát triển Frontend React, tích hợp Cognito + WebSocket |
| Tháng 3 | Tuần 9–12 | CI/CD GitHub Actions, CloudFront, kiểm thử, launch |
| Sau triển khai | Liên tục | Theo dõi CloudWatch, tối ưu chi phí, thêm tính năng |

---

### 6. Ước tính ngân sách

Mô hình **Pay-per-use** của AWS Serverless đảm bảo chi phí cực thấp khi lưu lượng ít:

| Dịch vụ | Ước tính/tháng |
|---|---|
| AWS Lambda | ~$0.00 (Free Tier: 1M requests/tháng) |
| Amazon API Gateway (HTTP) | ~$0.01 (2,000 requests) |
| Amazon API Gateway (WebSocket) | ~$0.02 (connection minutes) |
| Amazon DynamoDB (On-demand) | ~$0.00 (Free Tier: 25 GB) |
| Amazon S3 (Avatar + Frontend) | ~$0.05 |
| Amazon CloudFront | ~$0.01 (Free Tier: 1 TB/tháng) |
| Amazon Cognito | ~$0.00 (Free Tier: 50,000 MAU) |
| **Tổng** | **~$0.10–$0.20/tháng** |

> Chi phí thực tế có thể gần như $0 nhờ AWS Free Tier trong 12 tháng đầu.

---

### 7. Đánh giá rủi ro

#### Ma trận rủi ro

| Rủi ro | Mức ảnh hưởng | Xác suất | Giải pháp |
|---|---|---|---|
| Độ trễ WebSocket cao | Cao | Thấp | Chọn Region gần người dùng; tối ưu Lambda cold start |
| DynamoDB throttling | Trung bình | Thấp | On-demand billing tự scale; CloudWatch alarm |
| Chi phí vượt ngưỡng | Thấp | Thấp | AWS Budget Alert; TTL tự động xóa dữ liệu cũ |
| Lỗi đồng bộ game state | Cao | Trung bình | Kiến trúc authoritative backend; validate phía server |
| Mất kết nối WebSocket | Trung bình | Trung bình | Xử lý `wsDisconnect` và cơ chế reconnect phía client |

#### Kế hoạch dự phòng

- Nếu WebSocket API gặp sự cố: Fallback về polling HTTP API tạm thời.
- Sử dụng CloudFormation để rollback nhanh về phiên bản ổn định trước đó.
- CloudWatch Alarms gửi email khi Lambda errors hoặc DynamoDB throttle vượt ngưỡng.

---

### 8. Kết quả kỳ vọng

**Cải tiến kỹ thuật**
- Trải nghiệm game real-time mượt mà với độ trễ WebSocket dưới 100ms.
- Hệ thống tự mở rộng từ 1 đến hàng nghìn người chơi đồng thời mà không cần can thiệp thủ công.
- CI/CD hoàn chỉnh: mỗi lần push code lên `main` tự động deploy lên Production.

**Giá trị học thuật & nghề nghiệp**
- Hiểu sâu về kiến trúc Serverless event-driven trên AWS.
- Thực hành đầy đủ vòng đời phát triển phần mềm: Design → Code → Test → Deploy → Monitor.
- Nắm vững các dịch vụ AWS quan trọng: Cognito, API Gateway (HTTP + WebSocket), Lambda, DynamoDB, S3, CloudFront, SAM, CloudWatch.