---
title: "Worklog Tuần 8"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 1.8. </b> "
---

### Mục tiêu tuần 8:

* Đóng gói, kiểm tra lại toàn bộ ứng dụng trên môi trường Production Cloud.
* Hoàn thiện bộ tài liệu kỹ thuật (Sơ đồ ERD CSDL, Swagger API Docs, Tài liệu kiến trúc AWS Cloud).
* Xây dựng kịch bản Demo end-to-end, tổng kết và đánh giá kết quả thực hiện dự án Healthcare Web App.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2   | - Rà soát toàn bộ cấu hình hạ tầng AWS (EC2/ECS, RDS, S3, CloudWatch, Cognito) <br> - Đảm bảo các biến môi trường (.env) và chứng chỉ SSL/HTTPS hoạt động ổn định trên môi trường Production | 20/07/2026 | 20/07/2026 | [Tại đây](https://docs.aws.amazon.com/ec2/) |
| 3   | - Chuẩn hóa và tổng hợp tài liệu kỹ thuật dự án: <br>&emsp; + Sơ đồ thiết kế CSDL (ERD) 9 bảng trong RDS MySQL <br>&emsp; + Sơ đồ kiến trúc Cloud-native tổng quan hệ thống <br>&emsp; + Đóng gói file Postman Collection / Swagger API Spec cho các module | 21/07/2026 | 21/07/2026 | Document Kiến trúc Hệ thống  |
| 4   | - Xây dựng kịch bản kịch bản Demo hoàn chỉnh theo đúng Business Flow 6 bước: <br>&emsp; 1. Đăng ký/Đăng nhập qua AWS Cognito <br>&emsp; 2. Chatbot phân luồng triệu chứng y tế <br>&emsp; 3. Đặt lịch khám & Thanh toán phí khám online (`PREPAID`) <br>&emsp; 4. Bác sĩ đọc Báo cáo Tóm tắt AI trên Portal <br>&emsp; 5. Bác sĩ chẩn đoán & kê đơn <br>&emsp; 6. Lễ tân check-in & xuất hóa đơn thu tiền đợt 2 (`POSTPAID`) tại quầy | 22/07/2026 | 22/07/2026 | Document Kịch bản Demo |
| 5   | - Thực hành chạy Demo thử nghiệm, ghi hình video demo sản phẩm và chuẩn bị Slide báo cáo tổng kết dự án | 23/07/2026 | 23/07/2026 | [Tại đây](https://cloudjourney.awsstudygroup.com/) |
| 6   | - Tổng kết công việc 8 tuần, đánh giá các mục tiêu kỹ thuật đã hoàn thành và đề xuất các hướng phát triển nâng cấp trong tương lai | 24/07/2026 | 24/07/2026 | Báo cáo Tổng kết Dự án |


### Kết quả đạt được tuần 8:

* Triển khai và đóng gói thành công toàn bộ hệ thống Healthcare Web App trên nền tảng AWS Cloud, đảm bảo tính sẵn sàng cao và bảo mật với SSL/HTTPS.
* Hoàn thiện bộ tài liệu kỹ thuật chuẩn mực bao gồm: Sơ đồ ERD CSDL, sơ đồ kiến trúc các dịch vụ Cloud AWS, và danh mục API chi tiết trên Swagger.
* Xây dựng và thực hiện thành công kịch bản Demo End-to-End trôi chảy, chứng minh tính khả thi và mượt mà của Business Flow.
* Đánh giá dự án đạt đầy đủ các tiêu chuẩn kỹ thuật đề ra:
  * Xác thực người dùng an toàn qua **AWS Cognito**.
  * Phân luồng triệu chứng và tạo Báo cáo Tóm tắt Y khoa tự động qua **Amazon SageMaker**.
  * Chống trùng lịch (Race Condition) triệt để trên **AWS RDS MySQL** bằng Row-level Lock.
  * Tích hợp thanh toán linh hoạt 2 đợt (`PREPAID` online và `POSTPAID` tại quầy).
  * Giám sát hệ thống và log tập trung qua **AWS CloudWatch**.
* Hoàn thành đúng tiến độ 8 tuần báo cáo, sẵn sàng cho buổi nghiệm thu và bảo vệ dự án.

