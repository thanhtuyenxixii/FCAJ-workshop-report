---
title: "Worklog Tuần 4"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 1.4. </b> "
---
### Mục tiêu tuần 4:

* Khởi tạo và tích hợp hệ quản trị cơ sở dữ liệu quan hệ **Amazon RDS (PostgreSQL/MySQL)** để lưu trữ các dữ liệu có cấu trúc cho ứng dụng Web Healthcare (thông tin người dùng, lịch hẹn khám, thông tin bác sĩ).
* Khởi tạo và cấu hình cơ sở dữ liệu NoSQL **Amazon DynamoDB** nhằm lưu trữ các dữ liệu có tần suất đọc/ghi cao hoặc linh hoạt (lịch sử trò chuyện Chatbot, phiên làm việc/session của người dùng).
* Cấu hình phân vùng mạng an toàn (VPC Private Subnet, Subnet Groups) và Security Groups cho RDS nhằm đảm bảo cơ sở dữ liệu chỉ có thể truy cập từ Server Backend EC2.
* Tích hợp ORM (TypeORM/Prisma) vào ứng dụng Backend NestJS để kết nối, truy vấn dữ liệu từ Amazon RDS và AWS SDK cho DynamoDB.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu  |
| --- | --- | --- | --- | --- |
| 2   | - Khởi tạo **Amazon RDS Database Instance** (PostgreSQL/MySQL) trên AWS Console thuộc Private Subnet <br> - Cấu hình DB Subnet Group, thiết lập tài khoản quản trị Admin và kích hoạt tính năng tự động sao lưu (Automated Backups) | 22/06/2026 | 22/06/2026 | [Tại đây](<https://cloudjourney.awsstudygroup.com/>) |
| 3   | - Khởi tạo **Amazon DynamoDB Table** (ChatHistory, UserSessions) với Partition Key và Sort Key phù hợp <br> - Cấu hình chế độ Auto-Scaling/On-Demand Capacity để tối ưu hóa chi phí và hiệu năng xử lý cho dữ liệu Chatbot | 23/06/2026 | 23/06/2026 | [Tại đây](<https://cloudjourney.awsstudygroup.com/>) |
| 4   | - Cấu hình **Security Group cho RDS**: Chỉ cho phép nhận Inbound Traffic từ Security Group của EC2 Backend trên cổng 5432 (PostgreSQL) hoặc 3306 (MySQL) <br> - Thực hiện chuyển đổi (Migration) dữ liệu từ CSDL local lên Amazon RDS | 24/06/2026 | 24/06/2026 | [Tại đây](<https://cloudjourney.awsstudygroup.com/>) |
| 5   | - Tích hợp ORM (Prisma/TypeORM) vào NestJS Backend để kết nối với Amazon RDS <br> - Tích hợp AWS DynamoDB SDK vào Module Chatbot trong NestJS để lưu trữ và truy xuất lịch sử hội thoại real-time của bệnh nhân với Chatbot | 25/06/2026 | 25/06/2026 | [Tại đây](<https://docs.nestjs.com/techniques/database>) |
| 6   | - Tiến hành kiểm thử các thao tác CRUD trên cả RDS và DynamoDB từ môi trường Staging/EC2 <br> - Kiểm tra hiệu năng kết nối (Connection Pooling) và mã hóa dữ liệu At-Rest (AWS KMS) trên cả hai hệ quản trị CSDL | 26/06/2026 | 26/06/2026 | [Tại đây](<https://cloudjourney.awsstudygroup.com/>) |


### Kết quả đạt được tuần 4:

* Triển khai thành công **Amazon RDS** nằm gọn trong Private Subnet, đảm bảo an toàn tuyệt đối cho cơ sở dữ liệu y tế, tránh các nguy cơ tấn công trực tiếp từ Internet.
* Khởi tạo thành công **Amazon DynamoDB** phục vụ lưu trữ lịch sử tin nhắn Chatbot và session làm việc với tốc độ phản hồi cực nhanh (độ trễ milisecond).
* Hoàn thành việc chuyển đổi cấu trúc CSDL (Database Migration) và kết nối mượt mà ứng dụng Backend NestJS trên EC2 tới Amazon RDS thông qua thư viện ORM.
* Đảm bảo tính bảo mật nghiêm ngặt nhờ thiết lập Security Group cho phép truy cập CSDL theo quy tắc "nhất quán phụ thuộc" (chỉ EC2 Backend mới có quyền truy cập vào RDS).
* Bệnh nhân có thể trò chuyện với Chatbot và tra cứu lại lịch sử trao đổi, đồng thời các dữ liệu đặt lịch hẹn khám được lưu trữ an toàn, toàn vẹn trên đám mây AWS.
* Chuẩn bị đầy đủ hạ tầng dữ liệu và ứng dụng hoàn chỉnh để sẵn sàng cho Tuần 5 tích hợp hệ thống giám sát **AWS CloudWatch** và xử lý lỗi tập trung.