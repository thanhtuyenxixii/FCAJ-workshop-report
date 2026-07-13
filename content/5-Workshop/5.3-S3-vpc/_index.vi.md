---
title : "Truy cập S3 từ VPC"
date : 2024-01-01 
weight : 3
chapter : false
pre : " <b> 5.3. </b> "
---

Trong phần này, chúng ta sẽ thực hiện xây dựng (build) và triển khai (deploy) toàn bộ tài nguyên Backend Serverless lên AWS bằng **AWS SAM CLI**.

#### 1. Cấu trúc tài nguyên Backend

Mã nguồn backend nằm trong thư mục `backend/` bao gồm:
* **`template.yaml`:** Khai báo hạ tầng S3, DynamoDB, API Gateway, Lambda, SNS, Cognito, CloudFront và Step Functions.
* **`state-machine.asl.json`:** Định nghĩa máy trạng thái (Step Functions State Machine) để xử lý tuần tự: phân nhánh file -> OCR Textract -> AI evaluation -> Lưu trữ -> Gửi mail thông báo.
* **Các Lambda Function:**
  * `GetPresignedUrlHandler` (Java): Sinh S3 Presigned URL để upload file trực tiếp.
  * `RouterStoreHandler` (Java): Router điều phối API, lưu log DB ban đầu và đẩy tin nhắn vào SQS.
  * `JobStarterHandler` (Java): Lắng nghe SQS để kích hoạt Step Functions một cách bất đồng bộ.
  * `textract-parser` (Node.js): Trích xuất chữ thô từ kết quả OCR của Textract.
  * `AiEvaluatorHandler` (Java): Lấy API key từ SSM, gửi bài viết tới Gemini Flash 1.5 và lưu kết quả chi tiết vào S3.

---

#### 2. Xây dựng mã nguồn (Build)

Mở terminal tại thư mục gốc của dự án và chạy lệnh sau để SAM biên dịch mã nguồn Java và Node.js:

```bash
sam build
```

Quá trình biên dịch sẽ sử dụng Maven để tải các thư viện Java dependencies và Node.js npm để cài đặt gói cho parser Lambda. Khi kết xuất thành công, bạn sẽ nhận được thông báo `Build Succeeded`.

![SAM Build Thành công](/images/5-Workshop/5.3-S3-vpc/sam-build.jpg)

---

#### 3. Triển khai tài nguyên (Deploy)

Triển khai stack lên tài khoản AWS của bạn bằng lệnh dưới đây (thay thế địa chỉ email bằng email thực tế của bạn để nhận thông báo từ SNS):

```bash
sam deploy \
    --stack-name essay-scoring \
    --region ap-southeast-1 \
    --parameter-overrides GeminiApiKeyParam=/essay-scoring/gemini-api-key NotificationEmail=testuser@gmail.com \
    --no-confirm-changeset \
    --resolve-s3 \
    --capabilities CAPABILITY_IAM
```

##### Giải thích các tham số:
* --stack-name: Tên của CloudFormation stack là essay-scoring.
* --region: Vùng triển khai ap-southeast-1 (Singapore).
* --parameter-overrides: Truyền tham số cấu hình:
  * GeminiApiKeyParam: Trỏ tới khoá SSM đã tạo ở bước chuẩn bị.
  * NotificationEmail: Địa chỉ email nhận kết quả điểm số.
* --resolve-s3: Cho phép SAM tự động tạo S3 bucket lưu trữ mã nguồn đóng gói.
* --capabilities CAPABILITY_IAM: Cho phép SAM tạo các IAM Role tự động cho Lambda và Step Functions.

---

#### 4. Xem kết quả đầu ra (Outputs)

Sau khi deploy hoàn tất (khoảng 3-5 phút), CloudFormation sẽ hiển thị phần **Outputs** chứa các thông tin quan trọng. Hãy lưu lại các giá trị này để dùng cho bước cấu hình frontend tiếp theo:

* **`ApiUrl`:** Endpoint API Gateway dùng cho frontend gọi backend.
* **`CognitoUserPoolId`:** ID của User Pool quản lý tài khoản người dùng.
* **`CognitoUserPoolClientId`:** ID của Web Client dùng để xác thực Cognito từ trình duyệt.
* **`FrontendUrl`:** Domain CloudFront CDN được tạo để phân phối website.

![Màn hình CloudFormation Outputs](/images/5-Workshop/5.3-S3-vpc/CloudFormation.png)