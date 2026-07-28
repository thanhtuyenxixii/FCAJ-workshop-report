---
title: "Worklog Tuần 6"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 1.6. </b> "
---

### Mục tiêu tuần 6:

* Phát triển các tính năng nâng cao: Kết nối Backend NestJS với mô hình AI trên Amazon SageMaker để phân tích triệu chứng y tế và tạo Báo cáo Tóm tắt (Summary Report).
* Tích hợp cổng thanh toán trực tuyến (PayOS / VNPay / Stripe) để xử lý luồng đặt cọc/thanh toán phí khám cơ bản (`PREPAID`).
* Xây dựng giao diện Dashboard phân quyền (Role-Based Access Control) cho Bác sĩ và Lễ tân; tối ưu hóa cấu trúc Code và chuẩn hóa RESTful API.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2   | - Tích hợp Backend NestJS với mô hình AI chạy trên **Amazon SageMaker Endpoint** <br> - Phát triển logic phân loại 3 mức độ (Đỏ - Vàng - Xanh) dựa trên kết quả trả về từ AI và điều hướng luồng giao diện người dùng | 06/07/2026 | 06/07/2026 | [Tại đây](<https://docs.aws.amazon.com/sagemaker/>) |
| 3   | - Xây dựng Module AI Pre-consultation: Tự động trích xuất lịch sử đoạn chat để AI tổng hợp thành **Báo cáo Tóm tắt chuẩn y khoa** <br> - Đính kèm báo cáo vào lịch hẹn trong CSDL để gửi tới Portal của Bác sĩ | 07/07/2026 | 07/07/2026 | Document thiết kế Business Flow |
| 4   | - Tích hợp Cổng thanh toán trực tuyến (**PayOS / VNPay / Stripe**) vào NestJS <br> - **Thực hành:** Viết API khởi tạo giao dịch thanh toán phí khám cơ bản (`PREPAID`) và xử lý Webhook xác thực giao dịch tự động | 08/07/2026 | 08/07/2026 | Tài liệu tích hợp API Payment Gateway |
| 5   | - Xây dựng Portal dành cho Nhân viên Y tế với cơ chế **Role-Based Access Control (RBAC)** trong NestJS Guard: <br>&emsp; + **Doctor Dashboard:** Xem danh sách lịch hẹn, đọc Báo cáo Tóm tắt AI, nhập chẩn đoán cuối cùng và kê đơn thuốc <br>&emsp; + **Receptionist Dashboard:** Check-in bệnh nhân, tạo hóa đơn thu tiền đợt 2 (`POSTPAID`) tại quầy | 09/07/2026 | 09/07/2026 | [Tại đây](<https://docs.nestjs.com/guards>) |
| 6   | - Refactor cấu trúc Code, tối ưu hóa các DTO (Data Transfer Object) và Validate dữ liệu đầu vào <br> - Chuẩn hóa và đóng gói tài liệu API bằng **Swagger / OpenAPI Spec** | 10/07/2026 | 10/07/2026 | [Tại đây](<https://docs.nestjs.com/openapi/introduction>) |


### Kết quả đạt được tuần 6:
* Kết nối thành công NestJS với **Amazon SageMaker Endpoint**, cho phép trích xuất thông tin triệu chứng từ Chatbot và tự động tạo **Báo cáo Tóm tắt chuẩn y khoa** giúp bác sĩ tiết kiệm thời gian chuẩn bị trước phiên khám.
* Hoàn thiện luồng phân loại 3 cấp độ nguy hiểm (Đỏ: Cảnh báo khẩn cấp/Cấp cứu 115 - Xanh: Lời khuyên tự chăm sóc - Vàng: Điều hướng sang chọn Lịch khám).
* Tích hợp thành công cổng thanh toán trực tuyến (PayOS / VNPay / Stripe), tự động cập nhật trạng thái lịch hẹn từ `PENDING_PAYMENT` sang `CONFIRMED` ngay sau khi nhận Signal Webhook.
* Xây dựng giao diện và API phân quyền (RBAC) hoàn chỉnh: Bác sĩ xem được báo cáo AI và nhập kết quả khám; Lễ tân dễ dàng check-in bệnh nhân và xuất hóa đơn thu chi phí phát sinh/tiền thuốc đợt 2 tại quầy.
* Chuẩn hóa toàn bộ danh mục API hệ thống bằng **Swagger (OpenAPI)**, áp dụng validation chặt chẽ cho toàn bộ request đầu vào, nâng cao độ tin cậy và tính dễ bảo trì cho mã nguồn.



