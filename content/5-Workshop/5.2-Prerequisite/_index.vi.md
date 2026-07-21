---
title : "Chuẩn bị"
date : 2024-01-01
weight : 2
chapter : false
pre : " <b> 5.2 </b> "
---

### Mục tiêu

Trong bước này, chúng ta sẽ chuẩn bị "sân bãi" trước khi động vào bất kỳ dịch vụ AWS nào: một tài khoản AWS, một IAM user quản trị (không dùng root), AWS CLI đã cấu hình, và source code đã sẵn sàng để deploy ở các bước sau.

### 2.1. Tài khoản AWS & Region

1. Đăng nhập [AWS Console](https://console.aws.amazon.com/). Nếu chưa có tài khoản, đăng ký tại [aws.amazon.com](https://aws.amazon.com/) (cần thẻ tín dụng; workshop này chủ yếu trong free tier).
2. Góc trên bên phải Console, chọn region **Asia Pacific (Singapore) `ap-southeast-1`** — mọi bước sau (trừ WAF/ACM cho CloudFront) đều làm ở region này.

![region](/images/5-Workshop/5.2-Prerequisite/01-region.png)

### 2.2. Tạo IAM user quản trị (không dùng root)

Nguyên tắc bảo mật đầu tiên: **root chỉ dùng để tạo IAM user, sau đó cất đi**.

1. Console → tìm **IAM** → **Users** → **Create user**.
2. User name: `phim-admin` → tick **Provide user access to the AWS Management Console** → chọn **I want to create an IAM user** → đặt password.

![tạo iam user](/images/5-Workshop/5.2-Prerequisite/02-create-iam-user.png)

3. Permissions: chọn **Attach policies directly** → tick **`AdministratorAccess`** → **Next** → **Create user**.

![gắn policy](/images/5-Workshop/5.2-Prerequisite/03-attach-policy.png)

4. Bật MFA: IAM → Users → `phim-admin` → tab **Security credentials** → **Assign MFA device** → chọn Authenticator app → quét QR bằng Google Authenticator.

![assign mfa](/images/5-Workshop/5.2-Prerequisite/04-assign-mfa.png)

5. Đăng xuất root, đăng nhập lại bằng `phim-admin`. Mọi bước sau đều làm với user này.

### 2.3. Cài AWS CLI v2 và tạo Access Key

1. Tải AWS CLI v2 cho Windows: <https://awscli.amazonaws.com/AWSCLIV2.msi> → cài đặt → mở PowerShell mới:

```powershell
aws --version
# aws-cli/2.x.x Python/3.x.x Windows/10 exe/AMD64
```

![cli version](/images/5-Workshop/5.2-Prerequisite/05-cli-version.png)

2. Tạo access key cho CLI: IAM → Users → `phim-admin` → **Security credentials** → **Create access key** → chọn use-case **Command Line Interface (CLI)** → tạo và **tải file .csv về nơi an toàn** (chỉ hiện 1 lần).

![access key](/images/5-Workshop/5.2-Prerequisite/06-access-key.png)

3. Cấu hình CLI:

```powershell
aws configure
# AWS Access Key ID:     <dán access key>
# AWS Secret Access Key: <dán secret key>
# Default region name:   ap-southeast-1
# Default output format: json
```

![aws configure](/images/5-Workshop/5.2-Prerequisite/07-aws-configure.png)

4. Kiểm tra:

```powershell
aws sts get-caller-identity
```

Kết quả phải trả về `Account` (12 số — **ghi lại số này**, các bước sau gọi là `<ACCOUNT_ID>`) và `Arn` chứa `user/phim-admin`.

![sts get-caller-identity](/images/5-Workshop/5.2-Prerequisite/08-sts-get-caller-identity.png)

### 2.4. Chuẩn bị source code & dịch vụ ngoài

| Hạng mục | Yêu cầu |
|---|---|
| Node.js | v20+ (`node --version`) |
| Backend | `cd phim-be && npm ci` chạy không lỗi; file `src/lambda.js`, `src/lambdaCron.js`, `src/config/ssm.js`, `src/services/storageService.js` đã có trong source |
| Frontend | `phim-fe/` đã push lên **GitHub** (Amplify sẽ kết nối repo này); file `amplify.yml` đã có |
| MongoDB Atlas | Có sẵn cluster + connection string `mongodb+srv://...`; Network Access cho phép `0.0.0.0/0` (bảo vệ bằng TLS + password mạnh) |
| Upstash Redis | Có REDIS_URL dạng `rediss://...` (free tier) |
| Email test | 1–2 địa chỉ email bạn truy cập được (để verify SES và nhận cảnh báo SNS) |

### Kết quả mong đợi

- Đăng nhập Console bằng `phim-admin` (có MFA), region Singapore.
- `aws sts get-caller-identity` trả đúng account.
- `npm ci` ở `phim-be/` thành công; repo `phim-fe` sẵn trên GitHub.

### 🛠 Troubleshooting

| Lỗi | Nguyên nhân & cách xử lý |
|---|---|
| `aws` không phải lệnh hợp lệ | Mở PowerShell **mới** sau khi cài; kiểm tra PATH |
| `InvalidClientTokenId` khi gọi CLI | Access key gõ sai/đã xóa — tạo key mới và `aws configure` lại |
| `npm ci` lỗi node-gyp/sharp | Đảm bảo Node v20+; xóa `node_modules` rồi chạy lại |
