---
title: "Blog 3"
date: 2026-07-07
weight: 3
chapter: false
pre: " <b> 3.3. </b> "
---

# AWS Lambda MicroVMs: Khi Serverless Không Còn Đồng Nghĩa Với Stateless

Bài viết này chia sẻ thông tin và góc nhìn cá nhân về tính năng mới **AWS Lambda MicroVMs** được AWS ra mắt vào cuối tháng 6/2026. Đây là một bước đột phá lớn giải quyết bài toán giữ trạng thái (stateful) cho các ứng dụng Serverless, đặc biệt hữu ích đối với các bài toán về AI Coding Assistant, Data Analytics và Sandbox học tập trực tuyến.

Các điểm chính cần nắm:

* **Vấn đề giải quyết:** Đáp ứng nhu cầu chạy mã nguồn độc lập của người dùng cuối trong một môi trường sandbox an toàn, nhanh chóng và tiết kiệm chi phí (điều mà EC2, Container truyền thống hay Lambda stateless thông thường chưa tối ưu hoàn hảo).
* **Cơ chế hoạt động:** Mỗi MicroVM hoạt động như một Firecracker virtual machine riêng biệt. Khác biệt lớn nhất là MicroVM được *suspend* (tạm dừng) thay vì bị hủy khi phiên làm việc tạm dừng, và nhanh chóng *resume* (khôi phục) trạng thái RAM và ổ đĩa, giúp bảo toàn dữ liệu, thư viện đã cài mà không tốn thời gian khởi động lại từ đầu.
* **Quyền kiểm soát của Developer:** Lập trình viên có thể chủ động điều khiển vòng đời MicroVM (khởi tạo, suspend, resume, kết thúc) và được cung cấp HTTPS endpoint riêng hỗ trợ HTTP/2, gRPC và WebSocket mà không cần cấu hình load balancer.
* **Ứng dụng thực tế cho AI Agent:** Tạo ranh giới thực thi (execution boundary) cách ly hoàn toàn mã do LLM sinh ra khỏi môi trường chính của agent để bảo mật thông tin xác thực (credentials).
* **Tối ưu chi phí:** Chỉ tính phí compute khi MicroVM đang active; trong thời gian suspend chỉ tính phí lưu trạng thái vô cùng rẻ so với việc chạy EC2 liên tục 24/7.
* **Góc nhìn cá nhân:** AWS đã biến một mô hình hạ tầng phức tạp mà các team giỏi hay tự xây dựng thành một managed service đóng gói sẵn, mở rộng ranh giới của Serverless sang cả các bài toán stateful nặng đô.

Vì đây là chia sẻ thông tin công nghệ mới từ cá nhân, bạn có thể xem chi tiết bài đăng và tham gia thảo luận cùng cộng đồng tại link gốc bên dưới.

> Tham khảo bài viết gốc ở [Link][Link_Original]

[Link_Original]: https://www.facebook.com/groups/awsstudygroupfcj/permalink/2202883497143277

> Hình ảnh bài post
>
> ![Trích dẫn bài viết về Lambda MicroVMs](/images/3-Blogpost/Blog3.1.png)
>
> ![Cơ chế hoạt động của Lambda MicroVMs](/images/3-Blogpost/Blog3.2.png)
>
> ![Góc nhìn cá nhân và tài liệu tham khảo](/images/3-Blogpost/Blog3.3.png)
>
> ![Sơ đồ kiến trúc hoạt động của Lambda MicroVMs](/images/3-Blogpost/Blog3.4.png)
