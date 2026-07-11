---
title: "Worklog Tuần 7"
date: 2026-06-21
weight: 7
chapter: false
pre: " <b> 1.7. </b> "
---

### Mục tiêu tuần 7:

* Tìm hiểu kết nối thời gian thực qua Socket để làm chế độ chơi online giữa hai người (PvP), bắt đầu bằng việc tạo phòng và nhập mã phòng (Custom Room).
* Làm lại giao diện bản đồ đặt tàu để trông đẹp mắt, dễ nhìn và dễ thao tác hơn.
* Xây dựng hệ thống tìm trận đấu và phân chia làm 2 chế độ: Đấu thường (casual) và Đấu hạng (ranked).
* Tạo 7 huy hiệu (badge) tương ứng với 7 mức rank trong game: Đồng, Bạc, Vàng, Bạch kim, Kim cương, Cao thủ, Đô đốc cùng mức điểm yêu cầu chi tiết.
* Thêm một bảng hiển thị nhỏ (popup) trong trang cá nhân (Profile) để xem các mức rank hiện có và rank hiện tại của mình.
* Dùng thư viện Phaser để tạo các hiệu ứng chuyển động (animation) khi bắn trúng, bắn trượt, nổ tàu và thông báo kết quả thắng/thua.

### Công việc thực hiện trong tuần:

| Ngày | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| ---- | --------- | ------------ | --------------- | -------------- |
| 2 | - Triển khai kết nối Socket phục vụ truyền dữ liệu thời gian thực giữa hai người chơi <br> - Tạo các hàm quản lý phòng trên backend (`createRoom.js`, `joinRoom.js`); viết tính năng phòng tự chọn đơn giản (Custom Room), cho phép tạo mã phòng và nhập mã phòng để hai máy kết nối chơi với nhau | 15/06/2026 | 15/06/2026 | https://cloudjourney.awsstudygroup.com/ |
| 3 | - Cải thiện bố cục và phối màu giao diện bản đồ đặt tàu <br> - Viết lại giao diện xếp tàu, căn chỉnh màu sắc, lưới ô và các nút bấm trên màn hình đặt tàu để dễ nhìn hơn | 16/06/2026 | 16/06/2026 | https://cloudjourney.awsstudygroup.com/ |
| 4 | - Triển khai hệ thống ghép trận tự động và chia hàng chờ: Đấu thường (casual) và Đấu hạng (ranked) <br> - Viết hàm tìm trận tự động ở backend (`matchmakeRoom.js`); kết nối Socket ở phía client để ghép cặp người chơi vào hàng chờ phù hợp | 17/06/2026 | 17/06/2026 | https://cloudjourney.awsstudygroup.com/ |
| 5 | - Cấu hình cơ chế tính điểm đấu hạng (Rank Points - RP) và 7 mức rank từ Đồng đến Đô đốc <br> - Tạo 7 hình ảnh huy hiệu (badge) tương ứng cho các mức rank; viết các hàm tính điểm trên backend (`calculateRankDelta`, `buildRankUpdate`) trong dịch vụ `rankService.js`; tích hợp hiệu ứng chuyển động khi lên rank | 18/06/2026 | 18/06/2026 | https://cloudjourney.awsstudygroup.com/ |
| 6 | - Xây dựng giao diện hiển thị danh sách các mức rank trên trang cá nhân <br> - Tạo popup `RankDetails` trong trang cá nhân (`Profile.jsx`) để người chơi bấm vào xem danh sách 7 mức rank, điều kiện điểm số và xem rank hiện tại của bản thân | 19/06/2026 | 19/06/2026 | https://cloudjourney.awsstudygroup.com/ |
| 7 | - Thiết lập hiệu ứng chuyển động trong trận đấu bằng thư viện Phaser <br> - Viết các hiệu ứng động bằng Phaser trong `BattleEffectsLayer.jsx` cho các hành động: bắn hụt (`playMiss`), bắn trúng (`playHit`), nổ tàu (`playSunkFinisher`) và hiệu ứng lên rank (`RankUpScene`) trong `RankUpAnimation.jsx` | 20/06/2026 | 21/06/2026 | https://cloudjourney.awsstudygroup.com/ |

### Kết quả đạt được:

* **Chế độ chơi giữa 2 người (PvP):**
  * Tích hợp thành công kết nối Socket để người chơi tạo phòng (`createRoom.js`) và nhập mã phòng tự chọn (`joinRoom.js`) chơi thử với nhau.
  * Hoàn thành hệ thống tự động tìm trận (`matchmakeRoom.js`) và chia thành 2 chế độ: Đấu thường và Đấu hạng.

* **Làm mới giao diện bản đồ đặt tàu:**
  * Sửa lại màn hình xếp tàu với màu sắc hài hòa, lưới ô rõ ràng và dễ thao tác hơn.

* **Hệ thống cấp bậc (Rank):**
  * Thiết lập thành công 7 mức rank từ Đồng đến Đô đốc dựa trên số điểm RP yêu cầu (Đồng: 200, Bạc: 500, Vàng: 950, Bạch kim: 1500, Kim cương: 2200, Cao thủ: 3100, Đô đốc: 4000).
  * Viết xong các hàm tính điểm trên backend (`calculateRankDelta`, `buildRankUpdate`) và hiệu ứng chuyển động khi lên rank (`RankUpScene`).
  * Thêm thành công popup xem thông tin các mức rank tại trang cá nhân (`Profile.jsx`).

* **Hiệu ứng động trong trận đấu:**
  * Dùng thư viện Phaser để làm thành công các hiệu ứng chuyển động bắt mắt khi bắn hụt (`playMiss`), bắn trúng (`playHit`), nổ tàu (`playSunkFinisher`).
  * Tạo popup hiện thông báo Thắng/Thua sống động khi kết thúc trận đấu.
