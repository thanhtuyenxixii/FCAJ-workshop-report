---
title: "Worklog Tuần 5"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 1.5. </b> "
---

### Mục tiêu tuần 5:

* Tích hợp dịch vụ AWS CloudWatch để theo dõi (Monitoring) và ghi nhật ký hoạt động (Logging) cho toàn bộ ứng dụng Web Healthcare.
* Thiết lập các cơ chế cảnh báo tự động (Alarms) giúp phát hiện sớm sự cố hạ tầng và lỗi ứng dụng.
* Xây dựng bộ xử lý ngoại lệ toàn cục (Global Exception Filters) trong NestJS để đảm bảo hệ thống không bị crash khi gặp sự cố, tối ưu hóa tính ổn định và tích hợp AWS SES/SNS để gửi thông báo.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu  |
| --- | --- | --- | --- | --- |
| 2   | - Tìm hiểu dịch vụ **AWS CloudWatch** (Logs, Metrics, Alarms, Dashboards) <br> - Cấu hình CloudWatch Agent trên EC2/ECS để đẩy log hệ thống Backend NestJS và Server Web Next.js về CloudWatch Logs | 29/06/2026 | 29/06/2026 | [Tại đây](<https://docs.aws.amazon.com/cloudwatch/>) |
| 3   | - Tích hợp thư viện Logger (Winston/Pino) vào NestJS để cấu hình ghi log có cấu trúc (JSON Format) <br> - Đẩy log chi tiết về các giao dịch thanh toán (Stripe/VNPay) và log kết nối WebSocket Chatbot lên CloudWatch Log Streams | 30/06/2026 | 30/06/2026 | [Tại đây](<https://docs.nestjs.com/techniques/logger>) |
| 4   | - Thiết lập **CloudWatch Alarms** cảnh báo tự động khi: <br>&emsp; + Tải CPU/Memory của Server EC2/RDS vượt ngưỡng 80% <br>&emsp; + Tỷ lệ lỗi API trả về HTTP Status 5xx vượt mức cho phép <br> - Tạo Dashboard trực quan hóa hiệu năng hệ thống trên CloudWatch | 01/07/2026 | 01/07/2026 | [Tại đây](<https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/AlarmThatSendsEmail.html>) |
| 5   | - Phát triển cơ chế **Global Exception Filter** và **Http Exception Filter** trong NestJS để bắt lỗi tập trung <br> - Xử lý fallback an toàn khi gặp lỗi ngắt kết nối CSDL RDS hoặc khi cổng thanh toán bị gián đoạn nhằm đảm bảo trải nghiệm người dùng không bị gián đoạn đột ngột | 02/07/2026 | 02/07/2026 | [Tại đây](<https://docs.nestjs.com/exception-filters>) |
| 6   | - Cấu hình dịch vụ **AWS SES (Simple Email Service)** / **AWS SNS (Simple Notification Service)** <br> - **Thực hành:** Viết Module tự động gửi Email/SMS xác nhận lịch hẹn và mã QR check-in cho bệnh nhân sau khi đặt lịch thành công | 03/07/2026 | 03/07/2026 | [Tại đây](<https://docs.aws.amazon.com/ses/>) |


### Kết quả đạt được tuần 5:

* Tập trung và chuẩn hóa toàn bộ Log của ứng dụng (Next.js & NestJS) lên **AWS CloudWatch Logs**, giúp việc tra cứu vết lỗi (Debugging) trở nên dễ dàng và nhanh chóng.
* Thiết lập thành công hệ thống cảnh báo **CloudWatch Alarms** kết hợp thông báo qua Email khi máy chủ gặp sự cố quá tải hoặc phát sinh nhiều lỗi 5xx.
* Hoàn thiện bộ xử lý lỗi toàn cục (**Global Exception Filters**) ở Backend NestJS, giúp che giấu thông tin lỗi nhạy cảm của hệ thống, chỉ trả về thông báo thân thiện cho bệnh nhân.
* Đảm bảo tính sẵn sàng cao và ổn định cho hệ thống: Chatbot và ứng dụng Web vẫn vận hành trôi chảy ngay cả khi một số dịch vụ bên thứ ba (như cổng thanh toán) gặp sự cố tạm thời.
* Tích hợp thành công **AWS SES/SNS** để gửi email tự động xác nhận lịch hẹn, đính kèm thông tin bác sĩ và mã lịch hẹn ngay sau khi bệnh nhân thanh toán tiền khám cơ bản.
* Xây dựng được Dashboard quan sát trực quan (Monitoring Dashboard) cho phép quản trị viên theo dõi sức khỏe hệ thống theo thời gian thực (Real-time).
