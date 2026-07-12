---
title: "Blog 2"
date: 2026-07-02
weight: 2
chapter: false
pre: " <b> 3.2. </b> "
---

# Chia sẻ về giải pháp khắc phục hiện tượng Lambda Cold Start

Bài viết này chia sẻ một "trick" thú vị do AI gợi ý giúp khắc phục hiện tượng khởi động lạnh (Cold Start) khi sử dụng dịch vụ AWS Lambda cùng API Gateway, giúp cải thiện tốc độ phản hồi cho các request đầu tiên.

Các điểm chính cần nắm:

* Hiểu về hiện tượng Cold Start xảy ra khi Lambda không hoạt động trong thời gian dài và bị "ngủ đông".
* Sử dụng Amazon EventBridge để lên lịch kích hoạt các hàm Lambda định kỳ mỗi 5 phút nhằm giữ ấm.
* Giải pháp tối ưu số lượng scheduler bằng cách tạo một Lambda điều phối trung gian là `WarmUpController` để ping song song đến các Lambda nghiệp vụ khác.
* Kỹ thuật bỏ qua logic nghiệp vụ nặng ở các Lambda chính bằng cách kiểm tra điều kiện sự kiện (event hoặc `detail-type` từ EventBridge) ngay đầu handler để phản hồi lập tức.
* Mong muốn chia sẻ và thảo luận thêm cùng cộng đồng về các phương án tối ưu hóa hiệu năng Lambda khác.

Vì đây là chia sẻ kinh nghiệm thực tế của cá nhân, bạn có thể xem chi tiết bài đăng và các bình luận góp ý thêm của cộng đồng ở link gốc bên dưới.

> Tham khảo bài viết gốc ở [Link][Link_Original]

[Link_Original]: https://www.facebook.com/groups/awsstudygroupfcj/

> Hình ảnh bài post
>
> ![Lược đồ kiến trúc xử lý Cold Start](/images/3-Blogpost/Blog2.1.jpg)
>
> ![Cấu hình kiểm thử Lambda Warmup](/images/3-Blogpost/Blog2.2.jpg)