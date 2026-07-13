---
title : "Dọn dẹp tài nguyên"
date : 2024-01-01
weight : 6
chapter : false
pre : " <b> 5.6. </b> "
---


Để tránh phát sinh chi phí không mong muốn từ các dịch vụ AWS sau khi hoàn thành bài thực hành, hãy tiến hành dọn dẹp các tài nguyên theo quy trình dưới đây.

> [!CAUTION]
> CloudFormation sẽ **không thể xoá** các S3 bucket nếu chúng đang chứa dữ liệu. Bạn bắt buộc phải dọn sạch (empty) dữ liệu trong các bucket trước khi xoá stack.

---

#### Bước 1: Dọn dẹp dữ liệu trong các Amazon S3 Bucket

Sử dụng AWS CLI hoặc AWS Console để xoá dữ liệu trong 3 bucket của hệ thống:

```bash
# 1. Xoá file bài viết đã upload trong raw bucket
aws s3 rm s3://essay-scoring-raw-919968175298-ap-southeast-1 --recursive

# 2. Xoá file kết quả chấm điểm JSON trong result bucket
aws s3 rm s3://essay-scoring-result-919968175298-ap-southeast-1 --recursive

# 3. Xoá file giao diện website trong frontend bucket
aws s3 rm s3://essay-scoring-frontend-919968175298-ap-southeast-1 --recursive
```

*(Lưu ý: Thay thế tên bucket thực tế hiển thị trong phần Output CloudFormation của bạn).*

---

#### Bước 2: Xoá CloudFormation Stack

Sau khi các bucket đã trống, tiến hành xoá toàn bộ stack bằng lệnh sau trong thư mục dự án:

```bash
sam delete
```

Khi được hỏi xác nhận (Are you sure you want to delete the stack essay-scoring?), nhập y (Yes) để đồng ý. Hệ thống sẽ tự động xoá bỏ tất cả Lambda, DynamoDB, API Gateway, CloudFront, Cognito User Pool, và các IAM Role liên quan.

Hoặc bạn có thể truy cập vào [AWS CloudFormation Console](https://ap-southeast-1.console.aws.amazon.com/cloudformation/home?region=ap-southeast-1), chọn stack **essay-scoring** và nhấn **Delete**.

---

#### Bước 3: Xoá Gemini API Key trong SSM Parameter Store

Để xoá khoá bảo mật đã lưu trữ trên Parameter Store, chạy lệnh sau:

```bash
aws ssm delete-parameter --name "/essay-scoring/gemini-api-key"
```

Quá trình dọn dẹp đã hoàn tất!