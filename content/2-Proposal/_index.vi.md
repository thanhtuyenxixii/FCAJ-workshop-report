---
title: "Bản đề xuất"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

Tại phần này, bạn sẽ thấy bản tóm tắt đề xuất dự án phát triển hệ thống Y tế số, bao gồm mục tiêu, kiến trúc hạ tầng AWS Cloud, luồng nghiệp vụ và ước tính ngân sách vận hành.

# Smart Healthcare AI Triage & Appointment Booking Platform  
## Giải pháp Cloud-native hỗ trợ Sàng lọc Bệnh Y khoa qua AI và Quản lý Lịch hẹn Trực tuyến  

### 1. Tóm tắt điều hành  
Smart Healthcare Platform được thiết kế nhằm hiện đại hóa quy trình tiếp nhận, sàng lọc ban đầu và đặt lịch khám bệnh tại các phòng khám/bệnh viện. Hệ thống ứng dụng mô hình trí tuệ nhân tạo (AI) trên **Amazon SageMaker** để phân loại triệu chứng của bệnh nhân theo 3 cấp độ (Đỏ - Vàng - Xanh) và tự động tạo báo cáo tóm tắt chuẩn y khoa cho bác sĩ trước ca khám. Nền tảng được xây dựng theo kiến trúc Microservices/Cloud-native trên nền tảng **AWS Cloud** (Cognito, ECS/EC2, RDS MySQL, S3, CloudWatch), phục vụ hàng nghìn lượt truy cập đồng thời từ bệnh nhân, bác sĩ và đội ngũ lễ tân.  

### 2. Tuyên bố vấn đề  
*Vấn đề hiện tại*  
Các cơ sở y tế hiện nay thường gặp tình trạng quá tải tại quầy tiếp đón. Quy trình đặt lịch hẹn truyền thống qua điện thoại hoặc tại quầy dễ gây ra tình trạng trùng lịch (Double-booking), thời gian chờ đợi của bệnh nhân kéo dài. Bên cạnh đó, bác sĩ tốn nhiều thời gian hỏi lại từ đầu các triệu chứng cơ bản của bệnh nhân do thiếu thông tin chuẩn bị trước (Pre-consultation summary).  

*Giải pháp*  
Nền tảng cung cấp giải pháp toàn diện bao gồm:  
1. **Chatbot AI thời gian thực:** Giao tiếp với bệnh nhân qua WebSocket, thu thập triệu chứng và gửi tới **Amazon SageMaker** để phân loại mức độ nguy cấp và tổng hợp báo cáo y khoa.
2. **Hệ thống đặt lịch chống trùng slot:** Sử dụng cơ chế khóa dòng (**Pessimistic Locking**) trên **AWS RDS MySQL** để giải quyết triệt để bài toán Race Condition khi nhiều người cùng đặt một khung giờ.
3. **Thanh toán linh hoạt 2 đợt:** Hỗ trợ thanh toán online đợt 1 (tiền khám gốc qua VNPay/Stripe) và thanh toán đợt 2 (tiền thuốc/xét nghiệm phát sinh tại quầy).
4. **Bảo mật và Phân quyền (RBAC):** Quản lý định danh qua **AWS Cognito** và bảo mật dữ liệu y tế nhạy cảm bằng mã hóa **UUID**.

*Lợi ích và hoàn vốn đầu tư (ROI)*  
- Giảm đến 60% thời gian chờ đợi tại quầy nhờ quy trình check-in tự động bằng mã QR.
- Tăng 30% hiệu suất làm việc của Bác sĩ nhờ có sẵn báo cáo tóm tắt triệu chứng do AI tổng hợp.
- Triệt tiêu 100% rủi ro trùng lịch khám, nâng cao trải nghiệm và sự hài lòng của bệnh nhân.
- Tối ưu hóa chi phí hạ tầng nhờ khả năng tự động mở rộng (Auto-scaling) của AWS Cloud theo lưu lượng truy cập thực tế.  

### 3. Kiến trúc giải pháp  
Hệ thống sử dụng kiến trúc Web Multi-tier trên nền tảng AWS Cloud, chia làm 3 lớp chính: Frontend (Next.js), Backend Service (NestJS trên ECS/EC2), và AI Services (Amazon SageMaker).  



*Dịch vụ AWS sử dụng*  
- *Amazon Cognito*: Quản lý định danh, đăng nhập/đăng ký và phân quyền người dùng (Patient, Doctor, Receptionist, Admin).
- *AWS EC2 / ECS*: Triển khai ứng dụng Web Frontend (Next.js) và Backend REST API / WebSocket (NestJS).
- *Amazon SageMaker*: Endpoint lưu trữ và chạy mô hình AI phân tích triệu chứng y tế.
- *Amazon RDS (MySQL)*: Cơ sở dữ liệu quan hệ lưu trữ thông tin bệnh nhân, bác sĩ, lịch làm việc và hóa đơn.
- *Amazon S3*: Lưu trữ file tĩnh, ảnh đại diện, đơn thuốc và kết quả cận lâm sàng.
- *AWS CloudWatch*: Thu thập log hệ thống, giám sát hiệu năng CPU/Memory và gửi cảnh báo sự cố.
- *AWS SES / SNS*: Tự động gửi Email/SMS xác nhận lịch hẹn kèm mã QR check-in.  

*Thiết kế thành phần*  
- *Giao diện Bệnh nhân (Next.js)*: Cho phép chat với Bot, xem danh sách bác sĩ, chọn khung giờ và thanh toán online.
- *Giao diện Nhân viên Y tế (Next.js)*:  
  - **Doctor Portal:** Xem lịch hẹn trong ngày, đọc báo cáo tóm tắt AI, nhập kết quả chẩn đoán và đơn thuốc.
  - **Reception Portal:** Check-in bệnh nhân qua mã QR, xuất hóa đơn và thu tiền đợt 2 tại quầy.
- *Backend Microservices (NestJS)*: Xử lý Business Logic, WebSocket Gateway, tích hợp Cổng thanh toán và kết nối CSDL RDS.  

### 4. Triển khai kỹ thuật  
*Các giai đoạn triển khai*  
Dự án được triển khai trong vòng **8 tuần** (2 tháng) qua các giai đoạn:  
1. *Tuần 1 - 2*: Phân tích yêu cầu, thiết kế kiến trúc Cloud AWS, thiết kế CSDL và khởi tạo Base Source Code (Next.js + NestJS).
2. *Tuần 3 - 4*: Triển khai hạ tầng AWS (EC2/ECS, S3, RDS MySQL), tích hợp ORM Prisma/TypeORM và xử lý cơ chế chống trùng lịch.
3. *Tuần 5 - 6*: Tích hợp AWS CloudWatch, tích hợp mô hình AI trên Amazon SageMaker, hoàn thiện cổng thanh toán (PayOS/VNPay/Stripe) và Dashboard phân quyền (RBAC).
4. *Tuần 7 - 8*: Kiểm thử tải (Stress Test), kiểm thử E2E, tối ưu Indexing CSDL, đóng gói tài liệu Swagger API và tổng kết Demo.

*Yêu cầu kỹ thuật*  
- *Frontend*: Next.js 14, TailwindCSS, Socket.io-client, React Query.
- *Backend*: NestJS Framework, TypeORM/Prisma, TypeScript, Socket.io.
- *Database*: MySQL 8.0 trên AWS RDS (Private Subnet).
- *AI/ML*: Python, Amazon SageMaker Endpoints.
- *DevOps*: Docker, AWS CLI, GitHub Actions (CI/CD).

### 5. Lộ trình & Mốc triển khai  
- *Giai đoạn 1 (Tuần 1 - Tuần 2)*: Hoàn thiện thiết kế System Architecture & CSDL Schema.
- *Giai đoạn 2 (Tuần 3 - Tuần 4)*: Triển khai thành công hạ tầng AWS (RDS, S3, ECS) & API Core.
- *Giai đoạn 3 (Tuần 5 - Tuần 6)*: Tích hợp thành công SageMaker AI, Payment Gateway & CloudWatch.
- *Giai đoạn 4 (Tuần 7 - Tuần 8)*: Hoàn thành Stress Test, tối ưu UX/UI và nghiệm thu dự án. 

### 6. Ước tính ngân sách  
Chi phí hạ tầng được ước tính trên môi trường AWS Cloud cho giai đoạn thử nghiệm (MVP / Development Phase): 

*Chi phí hạ tầng AWS hàng tháng*  
- *AWS RDS (db.t3.micro - MySQL)*: ~15.00 USD/tháng (Single-AZ, 20 GB Storage).
- *AWS EC2 (t3.small - Backend & Frontend)*: ~14.00 USD/tháng (Chạy 24/7).
- *Amazon SageMaker (ml.t3.medium Endpoint)*: ~36.00 USD/tháng (Phục vụ Serverless/On-demand AI Inference).
- *Amazon S3 Standard*: ~0.50 USD/tháng (5 GB Data Storage & Transfer).
- *Amazon Cognito*: 0.00 USD/tháng (Miễn phí 50,000 MAU đầu tiên).
- *AWS CloudWatch*: ~2.00 USD/tháng (Metrics, Logs & Alarms).
- *Cổng thanh toán (PayOS/VNPay Sandbox)*: 0.00 USD (Môi trường Test miễn phí).

*Tổng chi phí hạ tầng Cloud*: ~67.50 USD/tháng (~810.00 USD/năm).

### 7. Đánh giá rủi ro  
*Ma trận rủi ro*  
- Trùng lịch khám do thao tác đồng thời (Race Condition): Ảnh hưởng cao, xác suất trung bình.
- Gián đoạn kết nối mô hình AI (SageMaker Timeout): Ảnh hưởng trung bình, xác suất thấp.
- Lộ thông tin y tế bệnh nhân (Data Privacy): Ảnh hưởng rất cao, xác suất thấp. 

*Chiến lược giảm thiểu*  
- *Race Condition*: Sử dụng Pessimistic Locking trực tiếp từ tầng Database.  
- *AI Timeout*: Xây dựng luồng Fallback — Nếu AI bị lỗi, hệ thống tự động chuyển sang luồng chọn bác sĩ thủ công truyền thống mà không làm gián đoạn trải nghiệm người dùng.  
- *Data Privacy*: Sử dụng chuỗi UUID thay cho ID tự tăng trên URL, ẩn danh thông tin bệnh nhân khi lưu log lên CloudWatch.  

### 8. Kết quả kỳ vọng  
- **Cải tiến kỹ thuật:** Tự động hóa 80% quy trình tiếp nhận và phân luồng bệnh nhân; đạt chỉ số sẵn sàng của hệ thống > 99.9% trên nền tảng AWS Cloud.  
- **Giá trị dài hạn:** Cung cấp nguồn dữ liệu chuẩn hóa cho công tác phân tích và dự báo dịch bệnh; hệ thống dễ dàng mở rộng cho chuỗi nhiều chi nhánh phòng khám trong tương lai.
