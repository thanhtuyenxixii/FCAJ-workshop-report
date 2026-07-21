---
title : "Tạo bucket S3 cho avatar"
date : 2024-01-01
weight : 4
chapter : false
pre : " <b> 5.4 </b> "
---

### Mục tiêu

Ở bước này, chúng ta sẽ tạo một bucket S3 **private tuyệt đối** để lưu avatar người dùng — không bật public bất kỳ lúc nào. Backend (driver `STORAGE_DRIVER=s3` trong `src/services/storageService.js`) sẽ upload object vào prefix `avatars/`; sau này người dùng đọc avatar qua CloudFront + OAC (cấu hình ở bước 10), chứ không đọc trực tiếp từ bucket.

### Các bước

1. Console → **S3** → **Create bucket**.
2. Điền:
   - **Bucket name:** `phim-avatars-<ACCOUNT_ID>` (thay `<ACCOUNT_ID>` bằng 12 số tài khoản — tên bucket phải duy nhất toàn cầu)
   - **Region:** `ap-southeast-1`
   - **Object Ownership:** giữ mặc định (ACLs disabled)
   - **Block Public Access settings:** giữ nguyên **Block *all* public access = ON** (cả 4 ô tick) ✔
   - **Default encryption:** giữ mặc định SSE-S3 (`Amazon S3 managed keys`)
3. → **Create bucket**.

![bucket name](/images/5-Workshop/5.4-S3-Bucket/01-bucket-name.png)

{{% notice warning %}}
Phần **Block Public Access** phải bật cả 4 ô — minh chứng bucket không public (tiêu chí bảo mật của thang điểm).
{{% /notice %}}

![block public access](/images/5-Workshop/5.4-S3-Bucket/02-block-public-access.png)

![default encryption](/images/5-Workshop/5.4-S3-Bucket/03-default-encryption.png)

4. Cấu hình CORS (cho phép trình duyệt gọi khi cần): mở bucket → tab **Permissions** → kéo xuống **Cross-origin resource sharing (CORS)** → **Edit** → dán:

```json
[
  {
    "AllowedHeaders": ["*"],
    "AllowedMethods": ["GET", "PUT", "POST"],
    "AllowedOrigins": ["http://localhost:3000", "https://*.amplifyapp.com", "https://*.cloudfront.net"],
    "ExposeHeaders": ["ETag"],
    "MaxAgeSeconds": 3000
  }
]
```

→ **Save changes**.

![cấu hình cors](/images/5-Workshop/5.4-S3-Bucket/04-cors-configuration.png)

{{% notice tip %}}
Sau bước 9/10 khi đã có domain Amplify/CloudFront chính thức, có thể thu hẹp `AllowedOrigins` về đúng 2 domain đó (least privilege).
{{% /notice %}}

### ✅ Kết quả mong đợi

- Bucket `phim-avatars-<ACCOUNT_ID>` tồn tại ở `ap-southeast-1`, cột **Access** hiển thị "Bucket and objects not public".
- Tab Permissions có CORS đã lưu.

### 🛠 Troubleshooting

| Lỗi | Xử lý |
|---|---|
| `Bucket name already exists` | Tên bucket toàn cầu — thêm hậu tố khác (vd `-workshop`) |
| Sau này upload bị `AccessDenied` | IAM Role của Lambda chưa có quyền `s3:PutObject` trên `avatars/*` — xem bước 6 |
| Ảnh không hiển thị trên web | Đúng thiết kế! Bucket private — phải đọc qua CloudFront OAC (bước 10) hoặc presigned URL |
