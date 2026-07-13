---
title : "Các bước chuẩn bị"
date : 2024-01-01 
weight : 2
chapter : false
pre : " <b> 5.2. </b> "
---
Để bắt đầu triển khai hệ thống Essay Scorer AI, bạn cần cài đặt một số công cụ lập trình dưới máy cá nhân và thực hiện một số cấu hình bảo mật trên tài khoản AWS của mình.

#### 1. Cài đặt các công cụ lập trình (Local Tools)

Đảm bảo máy tính của bạn đã được cài đặt đầy đủ các phần mềm sau:

* **AWS CLI:** Dùng để cấu hình thông tin truy cập tài khoản AWS. 
  * Cấu hình bằng lệnh: `aws configure` (nhập Access Key ID, Secret Access Key và Default Region là `ap-southeast-1`).
* **AWS SAM CLI:** Công cụ quản lý và triển khai hạ tầng Serverless.
  * Tải và cài đặt theo tài liệu chính thức của AWS. Kiểm tra bằng lệnh: `sam --version`.
* **Java Development Kit (JDK 17) & Maven:** Dùng để biên dịch code Java của các Lambda function.
  * Kiểm tra bằng lệnh: `java -version` và `mvn -version`.
* **Node.js (phiên bản LTS):** Dùng để chạy môi trường phát triển cục bộ và đóng gói mã nguồn frontend.
  * Kiểm tra bằng lệnh: `node -v` và `npm -v`.

---

#### 2. Khởi tạo Gemini API Key từ Google AI Studio

Hệ thống sử dụng mô hình ngôn ngữ lớn **Gemini 1.5 Flash** để chấm điểm bài viết. Bạn cần chuẩn bị khoá API (API Key):

1. Truy cập [Google AI Studio](https://aistudio.google.com/).
2. Đăng nhập bằng tài khoản Google của bạn và nhấn nút **Get API Key**.
3. Tạo một API Key mới và copy giá trị của nó.

![Get Gemini API Key](/images/5-Workshop/5.2-Prerequisite/gemini-api-key.png)

---

#### 3. Lưu trữ API Key an toàn trên AWS SSM Parameter Store

Để tránh lộ API Key trong mã nguồn, chúng ta sẽ lưu khoá này vào **AWS Systems Manager (SSM) Parameter Store** dưới dạng một chuỗi mã hoá (`SecureString`).

Mở terminal (PowerShell hoặc Bash) và chạy lệnh sau (thay thế giá trị API Key thực tế của bạn):

``bash
aws ssm put-parameter \
    --name "/essay-scoring/gemini-api-key" \
    --value "YOUR_GEMINI_KEY" \
    --type "SecureString" \
    --overwrite
``

![Create Parameter Store in AWS](/images/5-Workshop/5.2-Prerequisite/Parameter.png)
![Create Parameter Store in AWS](/images/5-Workshop/5.2-Prerequisite/Parameter-2.png)

> **Lưu ý quan trọng:** Tên tham số `/essay-scoring/gemini-api-key` phải khớp chính xác với cấu hình khai báo trong file mẫu `template.yaml` của AWS SAM.

---

#### 4. Email cá nhân để nhận thông báo từ AWS SNS

Chuẩn bị một địa chỉ email cá nhân (ví dụ: `testuser@gmail.com`). Địa chỉ này sẽ được truyền vào tham số triển khai để AWS Simple Notification Service (SNS) gửi thư báo điểm trực tiếp về hộp thư của bạn.