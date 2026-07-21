---
title : "Cấu hình email với Amazon SES"
date : 2024-01-01
weight : 5
chapter : false
pre : " <b> 5.5 </b> "
---

### Mục tiêu

Trong bước này, chúng ta sẽ cho backend gửi email (xác thực, quên mật khẩu, thông báo) qua **Amazon SES**. Vì dùng **SMTP interface** của SES nên chúng ta **không phải sửa code** — file `src/config/email.js` đã hỗ trợ cấu hình qua `SMTP_HOST`/`SMTP_PORT`/`EMAIL_USER`/`EMAIL_PASS`, chỉ cần điền đúng giá trị vào SSM.

### 5.1. Verify địa chỉ email gửi

1. Console → **Amazon SES** (region `ap-southeast-1`) → menu trái **Identities** → **Create identity**.
2. Chọn **Email address** → nhập địa chỉ email dùng làm người gửi (vd `your-email@gmail.com`) → **Create identity**.
3. Mở hộp thư → click link xác nhận trong email "Amazon Web Services – Email Address Verification Request".
4. Quay lại SES → Identities → trạng thái chuyển **Verified**.

![tạo ses identity](/images/5-Workshop/5.5-SES-Email/01-create-ses-identity.png)

{{% notice warning %}}
**SES Sandbox:** tài khoản mới ở chế độ sandbox — chỉ gửi **đến** các địa chỉ đã verify. Để test đủ luồng, verify thêm 1–2 địa chỉ email người nhận test.
{{% /notice %}}

### 5.2. Tạo SMTP credentials

1. SES → menu trái **SMTP settings** → ghi lại **SMTP endpoint**: `email-smtp.ap-southeast-1.amazonaws.com`, port `587` (STARTTLS).
2. Bấm **Create SMTP credentials** → AWS mở trang IAM tạo user chuyên gửi mail (tên mặc định `ses-smtp-user.xxx`) → **Create user**.
3. **Tải về / copy ngay** SMTP user name và SMTP password (chỉ hiện 1 lần).

### 5.3. Điền SMTP credentials vào SSM

Quay lại **Systems Manager → Parameter Store**, tạo/sửa 4 parameter (SecureString): `/phim/prod/SMTP_HOST`, `/phim/prod/SMTP_PORT` (=`587`), `/phim/prod/EMAIL_USER`, `/phim/prod/EMAIL_PASS`.

### 5.4. (Khuyến nghị) Xin production access

SES → **Account dashboard** → khung "Your account is in the sandbox" → **Request production access** → Mail type = `Transactional`, Website URL = domain FE, mô tả use-case, cách xử lý bounce/complaint. AWS thường duyệt trong ~24h.

### Kiểm tra nhanh đầu-cuối

Gửi thử một email qua ứng dụng (vd đăng ký hoặc quên mật khẩu) để xác nhận SMTP credentials hoạt động đúng đầu-cuối.

![gửi email test](/images/5-Workshop/5.5-SES-Email/02-send-test-email.png)
![nhận email test](/images/5-Workshop/5.5-SES-Email/03-receive-test-email.png)

### ✅ Kết quả mong đợi

- Ít nhất 1 identity **Verified**; có SMTP credentials; 4 parameter SMTP đã lưu trong SSM.

### 🛠 Troubleshooting

| Lỗi | Xử lý |
|---|---|
| Không nhận được mail verify | Kiểm tra Spam; bấm Resend trong SES |
| Gửi mail bị `554 Message rejected: Email address is not verified` | Đang ở sandbox mà người **nhận** chưa verify — verify địa chỉ nhận hoặc xin production access |
| `535 Authentication Credentials Invalid` | SMTP password ≠ IAM secret key — phải dùng đúng cặp credentials tạo từ nút "Create SMTP credentials" |
