---
title : "Tạo secrets với SSM Parameter Store"
date : 2024-01-01
weight : 3
chapter : false
pre : " <b> 5.3 </b> "
---

### Mục tiêu

Ở bước này, chúng ta sẽ đưa toàn bộ secrets của backend (connection string, JWT secret, thông tin email…) vào **SSM Parameter Store** dạng **SecureString** (mã hóa KMS) — thay vì để trong file `.env` như trước. Khi chạy trên Lambda với `USE_SSM=true`, file `src/config/ssm.js` sẽ tự nạp tất cả parameter dưới prefix `/phim/prod` vào `process.env` lúc cold-start, nên chúng ta không cần sửa lại code đọc biến môi trường.

{{% notice note %}}
**Quy tắc đặt tên:** `/phim/prod/<TÊN_BIẾN>` → thành `process.env.<TÊN_BIẾN>`. Ví dụ `/phim/prod/MONGODB_URI` → `process.env.MONGODB_URI`.
{{% /notice %}}

### Các bước

1. Console → tìm **Systems Manager** → menu trái **Parameter Store** → **Create parameter**.
2. Tạo parameter đầu tiên:
   - **Name:** `/phim/prod/MONGODB_URI`
   - **Tier:** Standard (miễn phí)
   - **Type:** **SecureString** — KMS key giữ mặc định `alias/aws/ssm`
   - **Value:** dán connection string `mongodb+srv://...` của Atlas
   - → **Create parameter**.

![tạo parameter mongodb](/images/5-Workshop/5.3-SSM-Parameters/01-create-parameter-mongodb.png)

3. Lặp lại cho các parameter sau (tất cả **SecureString**, lấy giá trị từ file `.env` hiện tại của `phim-be`):

| Name | Giá trị |
|---|---|
| `/phim/prod/MONGODB_URI` | Connection string MongoDB Atlas |
| `/phim/prod/JWT_SECRET` | Chuỗi bí mật ký JWT (≥ 32 ký tự ngẫu nhiên) |
| `/phim/prod/REDIS_URL` | URL Upstash Redis `rediss://...` |
| `/phim/prod/FRONTEND_URL` | Domain frontend — tạm điền `http://localhost:3000`, **quay lại sửa sau bước 9/10** |
| `/phim/prod/SMTP_HOST` | `email-smtp.ap-southeast-1.amazonaws.com` — *điền sau ở bước 5* |
| `/phim/prod/SMTP_PORT` | `587` — *điền sau ở bước 5* |
| `/phim/prod/EMAIL_USER` | SMTP username từ SES — *điền sau ở bước 5* |
| `/phim/prod/EMAIL_PASS` | SMTP password từ SES — *điền sau ở bước 5* |

![tạo parameter jwt](/images/5-Workshop/5.3-SSM-Parameters/02-create-parameter-jwt.png)
![tạo parameter frontend url](/images/5-Workshop/5.3-SSM-Parameters/03-create-parameter-frontend-url.png)
![tạo parameter smtp host](/images/5-Workshop/5.3-SSM-Parameters/04-create-parameter-smtp-host.png)
![tạo parameter email user](/images/5-Workshop/5.3-SSM-Parameters/05-create-parameter-email-user.png)
![tạo parameter email pass](/images/5-Workshop/5.3-SSM-Parameters/06-create-parameter-email-pass.png)

{{% notice tip %}}
4 parameter SMTP có thể tạo luôn với giá trị tạm `pending` rồi **Edit** lại sau khi làm xong bước 5 (SES), hoặc để bước 5 mới tạo — tùy bạn.
{{% /notice %}}

4. Kiểm tra bằng CLI:

```powershell
aws ssm get-parameters-by-path --path /phim/prod --with-decryption --query "Parameters[].Name"
```

![verify cli](/images/5-Workshop/5.3-SSM-Parameters/07-verify-cli.png)

### Kết quả mong đợi

- Tối thiểu `MONGODB_URI`, `JWT_SECRET`, `REDIS_URL`, `FRONTEND_URL` tồn tại dưới `/phim/prod`, type SecureString.
- Lệnh `get-parameters-by-path` trả đủ danh sách.

### 🛠 Troubleshooting

| Lỗi | Xử lý |
|---|---|
| `AccessDeniedException` khi gọi CLI | User `phim-admin` chưa đủ quyền hoặc sai region — kiểm tra `aws configure` |
| Tạo trùng tên | Name phải duy nhất; dùng nút Edit thay vì tạo mới |
| Lambda sau này không đọc được | Sai prefix — env `SSM_PREFIX` phải khớp `/phim/prod` (không có `/` cuối) |
