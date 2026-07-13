---
title: "Workshop"
date: 2024-01-01
weight: 5
chapter: false
pre: " <b> 5. </b> "
---
# Hệ thống chấm điểm bài luận Tiếng Anh tự động (AWS Serverless & Gemini AI)

#### Tổng quan

Trong bài thực hành này, chúng ta sẽ xây dựng và triển khai một **Hệ thống chấm điểm và đánh giá bài luận tiếng Anh tự động** sử dụng kiến trúc Serverless tiên tiến trên AWS kết hợp với sức mạnh của trí tuệ nhân tạo tạo sinh (Generative AI) từ Google Gemini API.

Hệ thống cho phép người dùng đăng ký/đăng nhập, tải lên các bài viết của họ dưới dạng văn bản thô `.txt`, hình ảnh bài viết chụp lại (`.png`, `.jpg`, `.jpeg`) hoặc tài liệu `.pdf`. Quy trình OCR xử lý chữ, gọi AI chấm điểm, ghi nhận dữ liệu và gửi thông báo email báo điểm hoàn toàn tự động và không máy chủ (Serverless).

#### Nội dung

1. [Tổng quan về workshop](5.1-Workshop-overview/)
2. [Chuẩn bị trước](5.2-Prerequiste/)
3. [Triển khai hạ tầng Backend (SAM & Step Functions)](5.3-Deploy-backend/)
4. [Đóng gói & Triển khai Frontend (CloudFront & S3)](5.4-Deploy-frontend/)
5. [Kiểm thử & Xác thực hệ thống (Textract & SNS Email)](5.5-Test-verify/)
6. [Dọn dẹp tài nguyên](5.6-Cleanup/)