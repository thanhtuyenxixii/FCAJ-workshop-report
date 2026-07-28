---
title: "Worklog Tuần 3"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 1.3. </b> "
---
### Mục tiêu tuần 3:

* Triển khai ứng dụng Web Healthcare lên hạ tầng đám mây AWS sử dụng kết hợp **Amazon EC2** cho Server tính toán và **Amazon S3** cho lưu trữ đối tượng.
* Khởi tạo và cấu hình tài nguyên mạng (VPC, Subnets, Security Groups) đảm bảo an toàn truy cập cho các máy chủ EC2.
* Tích hợp dịch vụ **Amazon S3** vào Backend (NestJS) để quản lý việc tải lên/tải về các tài nguyên như ảnh đại diện bác sĩ, hồ sơ bệnh án và mã QR check-in.
* Thiết lập **AWS API Gateway** (hoặc Nginx Reverse Proxy trên EC2) để định tuyến các yêu cầu (Routing) và bảo vệ hệ thống API Backend.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu  |
| --- | --- | --- | --- | --- |
| 2   | - Khởi tạo **EC2 Instance** (Ubuntu/Linux) trên AWS Console, tạo Key Pair để truy cập SSH <br> - Cấu hình **Security Group** cấp quyền truy cập Inbound cho các cổng SSH (22), HTTP (80) và HTTPS (443) | 15/06/2026 | 15/06/2026 | [Tại đây](<https://cloudjourney.awsstudygroup.com/>) |
| 3   | - Thiết lập môi trường chạy trên EC2: Cài đặt Docker, Docker Compose, Git và Node.js <br> - Kéo (Pull) bộ mã nguồn ứng dụng từ GitHub về EC2 và thực thi triển khai ứng dụng bằng Docker Container | 16/06/2026 | 16/06/2026 | [Tại đây](<https://cloudjourney.awsstudygroup.com/>) |
| 4   | - Tạo **S3 Bucket** lưu trữ tài nguyên y tế, cấu hình chính sách truy cập (Bucket Policy, CORS) <br> - Tích hợp AWS SDK v3 vào NestJS Backend để tạo API upload/download file ảnh bệnh án và mã QR đặt lịch hẹn | 17/06/2026 | 17/06/2026 | [Tại đây](<https://cloudjourney.awsstudygroup.com/>) |
| 5   | - Nghiên cứu và thiết lập **Amazon API Gateway** (hoặc Nginx Reverse Proxy trên EC2) để điều hướng traffic từ domain công cộng vào các dịch vụ Backend <br> - Cấu hình giới hạn tần suất gọi API (Rate Limiting) để chống tấn công từ chối dịch vụ | 18/06/2026 | 18/06/2026 | [Tại đây](<https://docs.aws.amazon.com/apigateway/>) |
| 6   | - Tiến hành kiểm thử kết nối giữa Frontend (Next.js), Backend (NestJS) trên EC2 và dịch vụ lưu trữ Amazon S3 <br> - Tối ưu hóa dung lượng lưu trữ trên S3 và kiểm tra phân quyền bảo mật IAM Role gắn cho EC2 Instance | 19/06/2026 | 19/06/2026 | [Tại đây](<https://cloudjourney.awsstudygroup.com/>) |


### Kết quả đạt được tuần 3:

* Khởi tạo và cấu hình thành công máy chủ **Amazon EC2**, thiết lập mượt mà các cổng bảo mật với Security Groups.
* Đóng gói và triển khai ứng dụng Web Healthcare (Next.js & NestJS) vận hành ổn định trên môi trường máy chủ EC2 thông qua Docker Containers.
* Tích hợp thành công **Amazon S3** vào hệ thống Backend, cho phép bệnh nhân và bác sĩ tải lên/xem các tài liệu y tế, hình ảnh bệnh án và mã QR an toàn.
* Thiết lập hệ thống cổng giao tiếp API (API Gateway / Nginx) giúp kiểm soát, định tuyến luồng dữ liệu truy cập và tăng cường tính bảo mật cho hệ thống Backend.
* Áp dụng **IAM Roles cho EC2 Instance** để tương tác an toàn với S3 mà không cần lưu cứng chìa khóa truy cập (Access Keys) trong mã nguồn.
* Chuẩn bị sẵn sàng hạ tầng tính toán và lưu trữ tĩnh để tiếp tục tích hợp Cơ sở dữ liệu đám mây (RDS/DynamoDB) ở Tuần 4.