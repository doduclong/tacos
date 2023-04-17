## Báo cáo của nhóm 6 ##
---

﻿Bước 1: phân rã dịch vụ

Nghiệp vụ ban đầu: tự động hóa viết comment cho blog

1. Nhận yêu cầu viết comment
1. lấy thông tin được gửi đến(fb của người yêu cầu, lượng comment, nội dung comment, bài viết, giãn cách thời gian,…..)
1. kiểm tra ID của tài khoản
1. nếu ID không tồn tại, hoặc tài khoản để wall ở dạng private thì dừng process
1. xác nhận từ chối dịch vụ
1. gửi thông báo “tài khoản không tồn tại, tài khoản chưa public hoặc tài khoản đã chặn bot”
1. kiểm tra id bài viết
1. nếu không tìm thấy bài viết, dừng process
1. xác nhận từ chối dịch vụ
1. gửi thông báo “bài viết không tồn tại hoặc chưa public”
1. xác nhận chấp nhận dịch vụ
1. sinh comment vào bài viết bằng số lượng comment theo nội dung đã yêu cầu
1. nếu comment bị gửi thống báo bị vô hiệu hóa, dừng process
1. xác nhận từ chối dịch vụ
1. gửi thông báo “comment bị vô hiệu hóa bởi lý do..” trích dẫn lý do từ thông báo được gửi về
1. xác nhận dịch vụ hoàn thành 
1. gửi thông báo dịch vụ đã hoàn thành

Bước 2: xác định các service

Tách biệt cách service bất khả tri và không bất khả tri:

- bất khả tri: nhận yêu cầu, kiểm tra, xác nhận từ chối dịch vụ, xác nhận chấp nhận dịch vụ, xác nhận hoàn thành dịch vụ, gửi thông báo
- không bất khả tri: còn lại

Các hành động không bất khả tri sẽ đượng phân loại thành một tập các chức năng sơ bộ của service và được nhóm vào các chức năng của service.

| Dịch vụ | Chức năng |
| ----- | -----|
| ![](readme_imgs/Aspose.Words.e39c383a-e8f4-482c-8236-b1e138061437.001.png)| Hành động lấy nội dung comment từ phía khách hàng được xây dựng dưới service Get Details. | 
| ![](readme_imgs/Aspose.Words.e39c383a-e8f4-482c-8236-b1e138061437.002.png) | Nhận yêu cầu từ người dùng(bao gồm nội dung comment, số lượng, ..) service Get Details.
| ![](readme_imgs/Aspose.Words.e39c383a-e8f4-482c-8236-b1e138061437.003.png) | Update lượng comment được yêu cầu lên blog của tài khoản yêu cầu được xây dựng dưới service Update Comments.

- xác định dịch vụ ultility : 

| Dịch vụ | Chức năng |
| ----- | -----|
| ![](readme_imgs/Aspose.Words.e39c383a-e8f4-482c-8236-b1e138061437.004.png) | Gửi thông báo tin nhắn tới người dùng và log lại thông tin đó |

- Khởi tạo một microservice là Verify Application với dịch vụ là Verify

| Dịch vụ | Chức năng |
| ----- | -----|
| ![](readme_imgs/Aspose.Words.e39c383a-e8f4-482c-8236-b1e138061437.005.png) | Xác nhận thông tin của comment|

Việc triển khai môi trường cho service này cần tính tự chủ cao và có thể bao gồm Rule service để đảm bảo yêu cầu về độ tin cậy.

- bắt đầu mô hình hóa dịch vụ: 

![](readme_imgs/Aspose.Words.e39c383a-e8f4-482c-8236-b1e138061437.006.png)

Sinh ra các resource như sau:

- /Process/
- /Application/
- /Comment/
- /Users/
- /Blog/
- **/Message/**

Liên kết resource với các service

\*Get Details

` `get+ /comment/: 

comment

![](readme_imgs/Aspose.Words.e39c383a-e8f4-482c-8236-b1e138061437.007.png)

\*Get Details

get+/ user/

users

![](readme_imgs/Aspose.Words.e39c383a-e8f4-482c-8236-b1e138061437.008.png)

\*Update Comments

post+/blog/ 

blog 

![](readme_imgs/Aspose.Words.e39c383a-e8f4-482c-8236-b1e138061437.009.png)

\*Send Message

post+/message/ 

notification

![](readme_imgs/Aspose.Words.e39c383a-e8f4-482c-8236-b1e138061437.010.png)

\*Verify

post/application/

veriy

![](readme_imgs/Aspose.Words.e39c383a-e8f4-482c-8236-b1e138061437.011.png)

------

Ngôn ngữ sử dụng: JAVA

Phiên bản sử dụng: >= 17

Công nghệ sử dụng: Spring 3.0

Tên thành phần | Loại thành phần | Mô tả chức năng | Bên hoạt động 
-----|-----|-----|-----|
Spring DATA Rest | Web | Biến các hàm trong repository thành endpoint, bỏ qua các controller|Server
Spring HAL Explorer | Web | Hiển thị giao diện xem các request dưới dạng HAL_JSON| Server 
Spring Web | Web | Cung cấp mô hình MVC cho bên client| Client
Spring Eureka Server | Cloud | Lưu thông tin của kho dịch vụ | Server, client
Spring Discovery Client | Cloud | Khám phá các dịch vụ của API| Client
Spring DATA JPA | Database | Tự động hóa công việc kết nối và xử lý với cơ sở dữ liệu| Server
MySQL Driver | Database | Kết nối với cơ sở dữ liệu MySQL | Server
Spring Thymleaf | Template | Nhúng dữ liệu và hiển thị trang Web | Client
Spring Test | Test | Phục vụ kiểm thử | Server, client
Lombok | Devtools | Giảm thiểu code cho các lớp thực thể | Server, client
Spring Devtools | Devtools | Đẩy nhanh quá trình phát triển dịch vụ | Server, client
