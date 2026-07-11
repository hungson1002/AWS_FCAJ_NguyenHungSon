---
title: "Worklog Tuần 8"
date: 2026-06-28
weight: 8
chapter: false
pre: " <b> 1.8. </b> "
---

### Mục tiêu tuần 8:

* Cải thiện và trang trí lại giao diện toàn bộ dự án để trông đẹp mắt, sinh động và chuyên nghiệp hơn.
* Tạo thêm hình ảnh cho 6 chiếc tàu chiến mới để người chơi có thêm nhiều sự lựa chọn khi sắp xếp hạm đội.
* Tích hợp đầy đủ âm thanh cho game: tiếng bắn trượt, tiếng bắn trúng tàu, tiếng nổ tàu, nhạc chiến thắng, nhạc thua cuộc, nhạc nền sảnh chờ và nhạc nền lúc chiến đấu.
* Làm thêm tính năng Cài đặt (Settings) cho phép người chơi tùy chỉnh âm lượng hoặc bật/tắt nhạc nền và âm thanh hiệu ứng game.
* Làm lại giao diện đấu PvP hiển thị nhiều thông tin của hai người chơi hơn và thêm tính năng nhắn tin chat trực tiếp trong trận đấu.

### Công việc thực hiện trong tuần:

| Ngày | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| ---- | --------- | ------------ | --------------- | -------------- |
| 2 | - Điều chỉnh màu sắc và bố cục CSS để làm đẹp giao diện trang chủ, sảnh chờ và phòng game <br> - Chỉnh sửa giao diện toàn bộ dự án, làm mới các nút bấm, khung viền và màu nền giúp dự án trông đẹp mắt hơn | 22/06/2026 | 22/06/2026 | https://cloudjourney.awsstudygroup.com/ |
| 3 | - Chuẩn bị hình ảnh cho 6 loại tàu chiến mới phù hợp với lưới tọa độ game <br> - Tạo hình ảnh cho 6 chiếc tàu chiến mới; thêm các ảnh tàu này vào danh sách lựa chọn của người chơi trước khi bắt đầu trận đấu | 23/06/2026 | 23/06/2026 | https://cloudjourney.awsstudygroup.com/ |
| 4 | - Tích hợp và điều khiển âm thanh thông qua các thư viện hỗ trợ (như Howler.js) <br> - Viết dịch vụ âm thanh `soundService.js` để tích hợp nhạc nền cho sảnh chờ (`menuMusic`) và nhạc nền khi chiến đấu (`battleMusic`) thông qua hàm `syncBackgroundMusic`; cài đặt các hiệu ứng tiếng bắn trượt, bắn trúng và nổ tàu bằng hàm `playSound` | 24/06/2026 | 24/06/2026 | https://cloudjourney.awsstudygroup.com/ |
| 5 | - Xây dựng thanh trượt điều chỉnh âm lượng và quản lý trạng thái bật/tắt âm thanh trong React <br> - Làm tính năng cài đặt (Settings); tạo khung điều chỉnh âm lượng thanh trượt cho nhạc nền và âm thanh hiệu ứng game thông qua hàm `setSoundSettings` và `getSoundSettings` | 25/06/2026 | 25/06/2026 | https://cloudjourney.awsstudygroup.com/ |
| 6 | - Thiết lập khu vực hiển thị thông tin người chơi trên màn hình thi đấu PvP <br> - Làm lại giao diện phòng đấu PvP, bổ sung các khu vực hiển thị thông tin chi tiết của hai bên (tên, cấp bậc rank, ảnh đại diện, tỷ số thắng thua) | 26/06/2026 | 26/06/2026 | https://cloudjourney.awsstudygroup.com/ |
| 7 | - Cấu hình truyền nhận tin nhắn chat thời gian thực qua kết nối Socket giữa các người chơi <br> - Tích hợp khung chat `PvpBattlePanels.jsx` và viết tính năng chat trực tiếp trong phòng đấu bằng cách phát tín hiệu Socket (`sendPvpSignal`) và cập nhật danh sách tin nhắn (`appendChatMessage`); chạy thử toàn bộ hệ thống | 27/06/2026 | 28/06/2026 | https://cloudjourney.awsstudygroup.com/ |

### Kết quả đạt được:

* **Cải thiện giao diện dự án:**
  * Toàn bộ giao diện trang chủ, sảnh chờ và phòng game được làm mới, đẹp mắt và chuyên nghiệp hơn rất nhiều.

* **Thêm 6 tàu chiến mới:**
  * Tạo xong hình ảnh và đưa vào game 6 loại tàu chiến mới, giúp người chơi có thêm nhiều lựa chọn đa dạng để sắp xếp.

* **Hệ thống âm thanh đầy đủ:**
  * Tích hợp thành công nhạc nền sảnh chờ, nhạc chiến đấu (`syncBackgroundMusic`), nhạc khi chiến thắng và nhạc khi thua cuộc.
  * Thêm thành công hiệu ứng âm thanh bắn trúng, bắn trượt và tiếng nổ tàu (`playSound`).

* **Khung cài đặt tùy chỉnh âm thanh:**
  * Hoàn thành tính năng cài đặt cho phép người dùng tùy chỉnh âm lượng hoặc tắt/bật nhạc nền, âm thanh hiệu ứng game bằng thanh trượt tiện lợi (`setSoundSettings`, `getSoundSettings`).

* **Giao diện PvP nâng cấp & Tính năng Chat:**
  * Giao diện PvP hiển thị đầy đủ thông tin chi tiết của cả hai đấu thủ (ảnh đại diện, tên, cấp bậc).
  * Thêm thành công khung chat (`PvpBattlePanels.jsx`) giúp hai người chơi có thể nhắn tin trò chuyện khi đang thi đấu bằng tín hiệu socket (`sendPvpSignal`) và cập nhật giao diện tin nhắn (`appendChatMessage`).
