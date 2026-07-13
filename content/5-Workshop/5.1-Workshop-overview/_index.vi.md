---
title : "Giới thiệu"
date : 2024-01-01 
weight : 1
chapter : false
pre : " <b> 5.1. </b> "
---

#### Kiến trúc Hệ thống

Hệ thống được xây dựng hoàn toàn trên nền tảng **Serverless** của AWS, giúp tự động co giãn theo lượng người dùng, tối ưu chi phí và không cần quản lý máy chủ.

![](/images/2-Proposal/AWS.jpeg.png)

#### Các thành phần chính của dự án

1. **Bảo mật & Định danh (Stage 1):**
   * **AWS Cognito User Pool:** Quản lý tài khoản đăng ký, đăng nhập và xác thực người dùng bằng JWT token.
   * **AWS IAM Role & Policies:** Tuân thủ nguyên tắc phân quyền tối thiểu (Least Privilege).
2. **Cổng API & Xử lý Ingestion (Stage 2):**
   * **Amazon API Gateway:** Cung cấp cổng REST API an toàn với Cognito Authorizer.
   * **S3 Presigned URL:** Tối ưu hóa băng thông bằng cách cho phép client upload file trực tiếp lên S3 mà không cần đi qua API Gateway.
   * **Amazon SQS:** Nhận sự kiện không đồng bộ để giảm tải cho API Gateway.
3. **Quy trình Phân tích AI (Stage 3):**
   * **AWS Step Functions:** Quản lý luồng chấm điểm tự động.
   * **Amazon Textract:** Nhận diện và trích xuất chữ từ hình ảnh bài viết hoặc file PDF.
   * **Google Gemini AI:** Đánh giá bài luận, cho điểm từ 0-100 và nhận xét chi tiết dựa trên prompt chuyên gia.
4. **Lưu trữ & Thông báo (Stage 4):**
   * **Amazon DynamoDB:** Lưu trữ trạng thái chấm điểm, điểm số tổng quan của từng bài viết.
   * **Amazon S3 (Result Bucket):** Lưu trữ file JSON chi tiết nhận xét của AI để tối ưu bộ nhớ DynamoDB.
   * **Amazon SNS:** Gửi email báo điểm trực tiếp tới người dùng khi hoàn tất.
5. **Phân phối Web (Stage 5):**
   * **Amazon CloudFront & S3 Website Hosting:** Tối ưu hóa phân phối ứng dụng React tĩnh toàn cầu qua giao thức HTTPS bảo mật.
