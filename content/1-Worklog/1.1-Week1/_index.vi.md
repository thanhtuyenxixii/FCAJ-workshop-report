---
title: "Worklog Tuần 1"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 1.1. </b> "
---

### Mục tiêu tuần 1:

* Nghiên cứu tổng quan về kiến trúc ứng dụng Web hiện đại và điện toán đám mây (Cloud Computing).
* Tìm hiểu các dịch vụ hạ tầng cốt lõi trên **AWS** (EC2, S3, RDS, DynamoDB, API Gateway, IAM, VPC) ứng dụng cho hệ thống Web Healthcare.
* Phân tích, lựa chọn mô hình kiến trúc phù hợp cho ứng dụng (Monolithic vs Microservices vs Serverless) và thiết kế sơ đồ tổng quan hạ tầng Cloud cho ứng dụng y tế.
* Chuẩn bị môi trường phát triển cục bộ (Local Environment) và khởi tạo dự án với Next.js (Frontend) và NestJS (Backend).

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu  |
| --- | --- | --- | --- | --- |
| 2   | - Nghiên cứu tổng quan về điện toán đám mây (IaaS, PaaS, SaaS) và lợi ích khi triển khai ứng dụng Web trên Cloud <br> - Phân tích mô hình kiến trúc Web 3 lớp (3-Tier Architecture: Presentation - Application - Database) ứng dụng trong dự án Healthcare | 01/06/2026 | 01/06/2026 | [Tại đây](<https://aws.amazon.com/what-is-cloud-computing/>) |
| 3   | - Tìm hiểu các dịch vụ cốt lõi của AWS: **EC2** (Server), **S3** (Lưu trữ file/mã QR/ảnh bệnh án), **RDS/DynamoDB** (CSDL), **API Gateway** <br> - Đánh giá ưu/nhược điểm giữa việc tự quản lý server trên EC2 với việc sử dụng dịch vụ quản lý hoàn toàn (Managed Services) | 02/06/2026 | 02/06/2026 | [Tại đây](<https://docs.aws.amazon.com/>) |
| 4   | - Nghiên cứu giải pháp an toàn thông tin trên Cloud: Quản lý truy cập với **AWS IAM**, phân vùng mạng an toàn với **AWS VPC** (Public/Private Subnets) <br> - Tìm hiểu các tiêu chuẩn bảo mật dữ liệu y tế và mã hóa dữ liệu (At-Rest và In-Transit với SSL/TLS) | 03/06/2026 | 03/06/2026 | [Tại đây](<https://docs.aws.amazon.com/iam/>) |
| 5   | - Cài đặt công cụ phát triển: **AWS CLI**, **Node.js**, **Docker**, cấu hình tài khoản AWS IAM <br> - Khởi tạo Repository trên GitHub và cấu hình cấu trúc thư mục dự án cho Frontend (Next.js) và Backend (NestJS) | 04/06/2026 | 04/06/2026 | [Tại đây](<https://docs.aws.amazon.com/cli/>) |
| 6   | - Phác thảo và vẽ sơ đồ kiến trúc hạ tầng Cloud (AWS Architecture Diagram) cho ứng dụng Web Healthcare <br> - Lập tài liệu phân tích yêu cầu kỹ thuật (Technical Requirements) và chốt luồng tính năng chính: Đặt lịch khám, tra cứu hồ sơ, thanh toán và Chatbot | 05/06/2026 | 05/06/2026 | [Tại đây](<https://aws.amazon.com/architecture/>) |


### Kết quả đạt được tuần 1:

* Nắm vững kiến trúc Web đa tầng và lý thuyết nền tảng về điện toán đám mây cùng các dịch vụ cơ bản của AWS.
* Hoàn thành sơ đồ kiến trúc hạ tầng Cloud (**AWS Architecture Diagram**) tổng quan cho hệ thống Web Healthcare, xác định rõ vị trí triển khai Frontend, Backend và Database.
* Hiểu rõ cơ chế phân vùng mạng an toàn (VPC, Public/Private Subnet) và phân quyền người dùng/dịch vụ bằng AWS IAM.
* Khởi tạo thành công môi trường phát triển cục bộ và bộ mã nguồn ban đầu (Monorepo/Polyrepo) cho **Next.js** và **NestJS** kết nối sẵn với GitHub.
* Chuẩn bị cho Tuần 2 triển khai xây dựng giao diện người dùng và API Backend lõi cho hệ thống đặt lịch khám bệnh.