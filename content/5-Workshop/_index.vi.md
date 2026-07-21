---
title: "Workshop"
date: 2024-01-01
weight: 5
chapter: false
pre: " <b> 5. </b> "
---

# Triển khai Web Xem Phim lên AWS Serverless

#### Tổng quan

Trong workshop này, chúng ta sẽ cùng triển khai một **nền tảng xem phim trực tuyến** hoàn chỉnh (Netflix-clone) — gồm backend Node.js/Express và frontend Next.js 15 — lên một kiến trúc **AWS serverless** hoàn toàn, thay vì phụ thuộc vào một PaaS đơn lẻ như trước.

Chúng ta sẽ lần lượt đưa backend lên **Lambda + API Gateway**, đưa frontend lên **Amplify**, rồi đặt một **CloudFront** distribution duy nhất phía trước có gắn **WAF** để vừa tăng tốc vừa chặn tấn công lớp 7. Avatar người dùng được lưu trên **S3**, secrets nằm trong **SSM Parameter Store**, email hệ thống gửi qua **SES**, tác vụ cron chạy hàng ngày bằng **EventBridge**, và toàn bộ hệ thống được giám sát/cảnh báo bằng **CloudWatch + SNS** — tất cả liên kết với nhau bằng **IAM** theo nguyên tắc quyền tối thiểu.

**13 dịch vụ AWS sử dụng:** Lambda · API Gateway · Amplify · CloudFront · WAF · ACM · S3 · SES · SNS · EventBridge Scheduler · CloudWatch · SSM Parameter Store · IAM.

**Chi phí ước tính:** ~$16–18/tháng (phần lớn trong free tier). Nhớ làm bước [Dọn dẹp](5.13-Cleanup/) sau khi demo xong để không phát sinh chi phí!

#### Nội dung

1. [Giới thiệu](5.1-Introduction/)
2. [Chuẩn bị](5.2-Prerequisite/)
3. [Tạo secrets với SSM Parameter Store](5.3-SSM-Parameters/)
4. [Tạo bucket S3 cho avatar](5.4-S3-Bucket/)
5. [Cấu hình email với Amazon SES](5.5-SES-Email/)
6. [Tạo IAM Role cho Lambda](5.6-IAM-Role/)
7. [Deploy backend lên Lambda + API Gateway](5.7-Lambda-APIGateway/)
8. [Cron hàng ngày với EventBridge Scheduler](5.8-EventBridge-Cron/)
9. [Deploy frontend với AWS Amplify](5.9-Amplify-Frontend/)
10. [CloudFront + ACM + WAF](5.10-CloudFront-WAF/)
11. [Giám sát & cảnh báo với CloudWatch + SNS](5.11-Monitoring/)
12. [Kiểm thử & đo lường](5.12-Testing/)
13. [Dọn dẹp tài nguyên](5.13-Cleanup/)
