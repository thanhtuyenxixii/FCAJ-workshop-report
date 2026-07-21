---
title : "Tạo IAM Role cho Lambda"
date : 2024-01-01
weight : 6
chapter : false
pre : " <b> 5.6 </b> "
---

### Mục tiêu

Ở bước này, chúng ta sẽ tạo IAM Role `phim-lambda-role` dùng chung cho 2 Lambda function (backend + cron), tuân theo **nguyên tắc quyền tối thiểu (Principle of Least Privilege)**: role này chỉ được ghi log, đọc/ghi đúng prefix `avatars/*` của đúng 1 bucket, và đọc đúng prefix `/phim/prod` trong SSM — không hơn, không kém.

### Các bước

1. Console → **IAM** → **Roles** → **Create role**.
2. **Trusted entity type:** AWS service · **Use case:** **Lambda** → **Next**.

![chọn trusted entity](/images/5-Workshop/5.6-IAM-Role/01-select-trusted-entity.png)

3. **Add permissions:** tick policy **`AWSLambdaBasicExecutionRole`** (cho phép ghi CloudWatch Logs) → **Next**.

![thêm permissions](/images/5-Workshop/5.6-IAM-Role/02-add-permissions.png)

4. **Role name:** `phim-lambda-role` → **Create role**.

![đặt tên, review, tạo](/images/5-Workshop/5.6-IAM-Role/03-name-review-create.png)

5. Mở role vừa tạo → tab **Permissions** → **Add permissions → Create inline policy** → tab **JSON** → dán (thay `<ACCOUNT_ID>` bằng account của bạn, sửa tên bucket nếu khác):

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AvatarBucket",
      "Effect": "Allow",
      "Action": ["s3:PutObject", "s3:GetObject", "s3:DeleteObject"],
      "Resource": "arn:aws:s3:::phim-avatars-<ACCOUNT_ID>/avatars/*"
    },
    {
      "Sid": "ReadSecrets",
      "Effect": "Allow",
      "Action": ["ssm:GetParametersByPath"],
      "Resource": "arn:aws:ssm:ap-southeast-1:<ACCOUNT_ID>:parameter/phim/prod*"
    }
  ]
}
```

![inline policy json](/images/5-Workshop/5.6-IAM-Role/04-inline-policy-json.png)

6. **Policy name:** `phim-app-access` → **Create policy**.

![role đã tạo](/images/5-Workshop/5.6-IAM-Role/05-role-created.png)

### Giải thích least-privilege (dùng cho phần Bảo mật của báo cáo)

| Statement | Cho phép | KHÔNG cho phép |
|---|---|---|
| `AvatarBucket` | Put/Get/Delete object **chỉ trong** `phim-avatars-<ACCOUNT_ID>/avatars/*` | Đụng bucket khác, xóa bucket, đổi policy, list toàn bộ S3 |
| `ReadSecrets` | Đọc parameter **chỉ dưới** `/phim/prod` | Đọc secrets dự án khác, ghi/xóa parameter |
| `AWSLambdaBasicExecutionRole` | Tạo log group/stream, ghi log | Đọc log dịch vụ khác |

Không cấp `ses:*` vì email đi qua SMTP credentials riêng (bước 5) — thêm một lớp tách quyền.

### ✅ Kết quả mong đợi

Role `phim-lambda-role` tồn tại, trust policy cho `lambda.amazonaws.com`, đúng 2 policy như ảnh chụp.

### 🛠 Troubleshooting

| Lỗi | Xử lý |
|---|---|
| Policy JSON báo lỗi validate | Kiểm tra đã thay `<ACCOUNT_ID>`; ARN không được chứa khoảng trắng |
| Lambda sau này `AccessDenied` khi đọc SSM | Sai region trong ARN (`ap-southeast-1`) hoặc sai prefix `/phim/prod` |
| Lambda `AccessDenied` khi ghi S3 | Object key không nằm dưới `avatars/` — driver luôn ghi `avatars/user_...`, kiểm tra tên bucket |
