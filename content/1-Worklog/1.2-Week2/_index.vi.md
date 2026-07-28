---
title: "Worklog Tuần 2"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 1.2. </b> "
---


### Mục tiêu tuần 2:

* Thiết kế cơ sở dữ liệu và xây dựng cấu trúc các Module cốt lõi cho Backend (NestJS) của ứng dụng Web Healthcare.
* Lập trình các API RESTful quan trọng: Đăng ký/Đăng nhập (Authentication với JWT), Quản lý thông tin bác sĩ, Đặt lịch khám và Quản lý hồ sơ bệnh nhân.
* Phát triển giao diện người dùng Frontend (Next.js) cho các luồng công việc chính: Trang chủ, Tra cứu bác sĩ/chuyên khoa, Biểu mẫu đặt lịch hẹn.
* Tích hợp kết nối API giữa Frontend và Backend, xử lý CORS, đồng bộ trạng thái dữ liệu (State Management) và Container hóa ứng dụng bằng Docker Compose để chạy cục bộ.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu  |
| --- | --- | --- | --- | --- |
| 2   | - Thiết kế sơ đồ quan hệ cơ sở dữ liệu (ERD) cho ứng dụng Y tế (Bệnh nhân, Bác sĩ, Khung giờ khám, Lịch hẹn) <br> - Khởi tạo cấu trúc các Module, Controller và Service trong NestJS | 08/06/2026 | 08/06/2026 | [Tại đây](<https://cloudjourney.awsstudygroup.com/>) |
| 3   | - Xây dựng Module Authentication trong NestJS (sử dụng Passport.js và JWT) phục vụ xác thực người dùng <br> - Phát triển các RESTful API cốt lõi: Danh sách bác sĩ, Tạo lịch hẹn mới, Cập nhật trạng thái lịch hẹn | 09/06/2026 | 09/06/2026 | [Tại đây](<https://docs.nestjs.com/>) |
| 4   | - Xây dựng giao diện ứng dụng Web bằng Next.js và TailwindCSS <br> - Thiết kế các trang chức năng: Trang chủ tìm kiếm bác sĩ, Trang thông tin chi tiết dịch vụ khám, Form điền thông tin bệnh nhân và chọn khung giờ | 10/06/2026 | 10/06/2026 | [Tại đây](<https://nextjs.org/docs>) |
| 5   | - Tích hợp gọi API từ Next.js sang NestJS Backend bằng Axios/Fetch API <br> - Cấu hình middleware xử lý CORS, kiểm tra quyền truy cập (Guards/Protected Routes) và lưu trữ Token an toàn ở Client | 11/06/2026 | 11/06/2026 | [Tại đây](<https://cloudjourney.awsstudygroup.com/>) |
| 6   | - Viết file Dockerfile cho ứng dụng Frontend Next.js và Backend NestJS <br> - Tạo file docker-compose.yml để đóng gói toàn bộ ứng dụng và khởi chạy môi trường Web đầy đủ ở local | 12/06/2026 | 12/06/2026 | [Tại đây](<https://docs.docker.com/>) |


### Kết quả đạt được tuần 2:

* Hoàn thành sơ đồ cơ sở dữ liệu ERD chi tiết cho hệ thống Web Healthcare, định hình rõ cấu trúc dữ liệu cho bệnh nhân và bác sĩ.
* Xây dựng thành công bộ API RESTful cốt lõi ở Backend NestJS hỗ trợ xác thực bảo mật bằng JWT và xử lý logic đặt lịch khám.
* Hoàn thiện giao diện ứng dụng Web tương tác ở Frontend với Next.js, đáp ứng tốt trải nghiệm người dùng khi tra cứu và chọn lịch hẹn.
* Tích hợp thành công luồng gọi API end-to-end từ Frontend đến Backend, xử lý mượt mà các trường hợp phân quyền và lỗi CORS.
* Container hóa thành công cả ứng dụng Frontend và Backend với Docker Compose, sẵn sàng cho việc triển khai lên hạ tầng AWS ở Tuần 3.