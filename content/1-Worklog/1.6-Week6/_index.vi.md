---
title: "Worklog Tuần 6"
date: 2026-06-14
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
---

### Mục tiêu tuần 6:

* Tìm hiểu và cấu hình dịch vụ AWS Cognito để kết nối với dự án, thiết kế giao diện đăng ký và đăng nhập liên kết với Cognito để làm tính năng này.
* Cấu hình các phần cần thiết trên Cognito để hỗ trợ đăng nhập bằng Google và Facebook.
* Dùng AWS Lambda và Amazon API Gateway để làm phần serverless phía backend.
* Thiết lập cơ sở dữ liệu Amazon DynamoDB để lưu lại thông tin người dùng.
* Tạo hình ảnh của 4 chiếc tàu chiến đầu tiên để thêm vào chế độ đấu với máy (Player vs AI).
* Thử nghiệm và sửa các lỗi liên quan đến việc xoay tàu, đặt tàu và kéo thả tàu trên bàn chơi.

### Công việc thực hiện trong tuần:

| Ngày | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| ---- | --------- | ------------ | --------------- | -------------- |
| 2 | - Thiết lập quản lý tài khoản bằng dịch vụ AWS Cognito (User Pools) <br> - Tạo và cấu hình Cognito User Pool; thiết kế các màn hình Đăng ký (`Register.jsx`) và Đăng nhập (`Login.jsx`); cài đặt Amplify SDK để kết nối giao diện với Cognito và gọi các hàm xác thực (`registerUser`, `confirmRegister`, `loginUser`) | 08/06/2026 | 08/06/2026 | https://cloudjourney.awsstudygroup.com/ |
| 3 | - Tích hợp đăng nhập bằng Google và Facebook thông qua Cognito <br> - Thiết lập App Registration trên Google Cloud Console và Facebook Developers; cấu hình Identity Providers trên Cognito và gọi hàm `loginWithSocialProvider` để bật đăng nhập Google và Facebook | 09/06/2026 | 09/06/2026 | https://cloudjourney.awsstudygroup.com/ |
| 4 | - Triển khai serverless backend sử dụng Lambda và API Gateway <br> - Tạo API Gateway (REST API); viết và cấu hình hàm Lambda để xử lý thông tin người chơi (`getUser.js`) phục vụ cho phía Frontend | 10/06/2026 | 10/06/2026 | https://cloudjourney.awsstudygroup.com/ |
| 5 | - Thiết lập cơ sở dữ liệu DynamoDB để lưu trữ và quản lý dữ liệu người dùng <br> - Tạo bảng DynamoDB để lưu thông tin người chơi; viết Lambda Function (`createUser.js`) kết nối với DynamoDB để tự động đồng bộ và lưu thông tin người dùng ngay sau khi xác thực thành công | 11/06/2026 | 11/06/2026 | https://cloudjourney.awsstudygroup.com/ |
| 6 | - Thiết kế và xử lý hình ảnh các tàu chiến phù hợp với kích thước game <br> - Tạo hình ảnh cho 4 chiếc tàu chiến đầu tiên; thêm các ảnh tàu này vào code và hiển thị trên màn hình đấu với máy (Player vs AI) | 12/06/2026 | 12/06/2026 | https://cloudjourney.awsstudygroup.com/ |
| 7 | - Cấu hình xử lý tọa độ lưới đấu và cơ chế kéo thả hình ảnh (Drag and Drop) <br> - Chạy thử và sửa lỗi các phần: xoay tàu (`rotateShip` / `handleRotate`), kéo thả tàu (`dragStart`, `dragOver`, `drop`) và đặt tàu vào ô lưới (`isValidPlacement`), giúp việc đặt tàu mượt mà hơn | 13/06/2026 | 14/06/2026 | https://cloudjourney.awsstudygroup.com/ |

### Kết quả đạt được:

* **Tích hợp thành công Xác thực người dùng (AWS Cognito):**
  * Thiết lập thành công Cognito User Pool kết nối với Frontend.
  * Thiết kế xong giao diện Đăng ký (`Register.jsx`) và Đăng nhập (`Login.jsx`), thực hiện liên kết và chạy các hàm API của Amplify (`registerUser`, `confirmRegister`, `loginUser`).
  * Tích hợp thành công đăng nhập bằng tài khoản thông thường và đăng nhập qua Google, Facebook (`loginWithSocialProvider`).

* **Xây dựng hệ thống Serverless API Backend:**
  * Cấu hình xong API Gateway kết nối với AWS Lambda (`getUser.js`) để nhận và xử lý các yêu cầu lấy thông tin hồ sơ người chơi.

* **Lưu trữ dữ liệu người dùng với DynamoDB:**
  * Tạo bảng lưu thông tin người chơi trên DynamoDB.
  * Hoàn thiện Lambda Function (`createUser.js`) chạy dưới dạng Post Confirmation Trigger để tự động lưu thông tin người dùng mới vào database ngay sau khi đăng ký hoặc đăng nhập xã hội thành công.

* **Tạo và thêm ảnh tàu chiến:**
  * Tạo xong và đưa vào sử dụng hình ảnh của 4 chiếc tàu chiến đầu tiên.
  * Hiển thị thành công các ảnh tàu này trên giao diện chế độ đấu với máy (Player vs AI).

* **Sửa các lỗi đặt tàu và xoay tàu trên bàn chơi:**
  * Sửa lỗi xoay tàu (xoay ngang dọc không bị lệch lưới hay đè lên tàu khác).
  * Sửa lại tính năng kéo thả và đặt tàu trên lưới ô để người chơi xếp tàu dễ dàng hơn trước khi bắt đầu trận.
