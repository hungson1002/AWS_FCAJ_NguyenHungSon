---
title: "Worklog Tuần 9"
date: 2026-07-05
weight: 9
chapter: false
pre: " <b> 1.9. </b> "
---

### Mục tiêu tuần 9:

* Đưa giao diện trang web lên internet bằng dịch vụ Amazon S3 và Amazon CloudFront.
* Nén dung lượng các ảnh huy hiệu rank và ảnh tàu chiến để trang web tải nhanh và mượt mà hơn.
* Tạo tệp cấu hình tự động hóa CI/CD bằng GitHub Actions để tự động cập nhật web mỗi khi đẩy code lên nhánh `develop`.
* Chạy thử nghiệm lần lượt các tính năng hiện tại và khắc phục các lỗi còn tồn đọng trong dự án.
* Thiết kế và làm lại giao diện (UI) hoàn toàn mới cho toàn bộ các màn hình của game với phong cách đẹp mắt, hiện đại và hấp dẫn hơn.

### Công việc thực hiện trong tuần:

| Ngày | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| ---- | --------- | ------------ | --------------- | -------------- |
| 2 | - Triển khai lưu trữ trang web tĩnh trên Amazon S3 và phân phối qua CDN CloudFront <br> - Tạo S3 Bucket để chứa code giao diện Frontend; cấu hình CloudFront để phân phối trang web và thiết lập bảo mật | 29/06/2026 | 29/06/2026 | https://cloudjourney.awsstudygroup.com/ |
| 3 | - Tối ưu hóa và nén hình ảnh sang định dạng WebP để giảm dung lượng tải trang <br> - Thực hiện nén dung lượng toàn bộ các ảnh huy hiệu rank và ảnh tàu chiến; thay thế các ảnh cũ bằng ảnh mới đã tối ưu | 30/06/2026 | 30/06/2026 | https://cloudjourney.awsstudygroup.com/ |
| 4 | - Xây dựng hệ thống tự động hóa CI/CD bằng GitHub Actions để đẩy code lên AWS <br> - Viết tệp cấu hình CI/CD (`deploy-frontend.yml`) để tự động chạy build và cập nhật giao diện web lên S3, đồng thời xóa cache trên CloudFront khi có code mới đẩy lên nhánh `develop` | 01/07/2026 | 01/07/2026 | https://cloudjourney.awsstudygroup.com/ |
| 5 | - Thiết lập kịch bản và chạy thử nghiệm để kiểm tra toàn bộ tính năng của game <br> - Chạy thử nghiệm toàn bộ hệ thống (đăng nhập Cognito, ghép trận PvP, tính điểm rank, âm thanh) và sửa các lỗi phát sinh trong game | 02/07/2026 | 02/07/2026 | https://cloudjourney.awsstudygroup.com/ |
| 6 | - Ứng dụng phong cách thiết kế hiện đại và màu sắc mới cho giao diện game <br> - Thiết kế và thay đổi giao diện hoàn toàn mới cho các trang: Trang chủ (Home), Trang cá nhân (Profile) và màn hình xếp tàu | 03/07/2026 | 03/07/2026 | https://cloudjourney.awsstudygroup.com/ |
| 7 | - Cấu hình hiển thị responsive giao diện mới tương thích trên di động và máy tính bảng <br> - Hoàn thiện giao diện mới bắt mắt cho phòng đấu PvP; tối ưu hiển thị responsive trên các màn hình di động và kiểm thử lần cuối | 04/07/2026 | 05/07/2026 | https://cloudjourney.awsstudygroup.com/ |

### Kết quả đạt được:

* **Triển khai trang web lên môi trường chạy thực tế:**
  * Đưa thành công giao diện Frontend lên S3 Bucket và cấu hình CloudFront để người dùng truy cập trực tiếp qua internet với tốc độ phản hồi nhanh.

* **Tối ưu dung lượng hình ảnh:**
  * Nén toàn bộ ảnh tàu chiến và ảnh huy hiệu rank sang định dạng tối ưu WebP, giúp giảm dung lượng tải trang và tăng tốc độ hiển thị.

* **Tự động hóa triển khai (CI/CD GitHub Actions):**
  * Tạo thành công tệp workflow (`deploy-frontend.yml`) giúp tự động build, đẩy mã nguồn lên S3 và xóa cache CloudFront mỗi khi có code mới đẩy lên nhánh `develop` hoặc `main`.

* **Kiểm thử và sửa lỗi:**
  * Chạy kiểm thử toàn bộ các tính năng lớn nhỏ, sửa dứt điểm các lỗi phát sinh liên quan đến luồng kết nối và đồng bộ hóa trận đấu.

* **Làm mới giao diện hoàn toàn (New UI):**
  * Thay thế giao diện cũ bằng giao diện mới hiện đại, phối màu đẹp mắt và có nhiều hiệu ứng chuyển động hấp dẫn ở sảnh chờ, trang cá nhân và phòng game PvP.
