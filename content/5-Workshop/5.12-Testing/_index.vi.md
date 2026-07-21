---
title : "Kiểm thử & đo lường"
date : 2024-01-01
weight : 12
chapter : false
pre : " <b> 5.12 </b> "
---

### Mục tiêu

Đã dựng xong toàn bộ hạ tầng, giờ là lúc chúng ta chứng minh hệ thống thật sự chạy end-to-end: request thành công, xử lý lỗi đúng, avatar vào S3, email SES gửi được, cron chạy, alarm bắn email. Mỗi mục dưới đây là một bằng chứng cụ thể chúng ta có thể đưa vào phần "Kiểm thử & đo lường" của báo cáo.

### 12.1. API hoạt động (happy path)

```powershell
curl https://<dist-id>.cloudfront.net/api/health-check
curl "https://<dist-id>.cloudfront.net/api/movies?limit=2"
```

Cả 2 lệnh phải trả JSON.

### 12.2. Kiểm thử lỗi (error handling)

```powershell
curl -i https://<dist-id>.cloudfront.net/api/khong-ton-tai   # mong đợi 404
curl -i https://<dist-id>.cloudfront.net/api/favorites        # mong đợi 401 khi không gửi token
```

Backend phải xử lý lỗi chuẩn (JSON có cấu trúc), không crash.

### 12.3. Upload avatar → S3

1. Mở web (domain CloudFront/Amplify) → đăng nhập → **Hồ sơ → đổi avatar** → upload một ảnh.
2. Avatar mới hiển thị ngay trên web (URL dạng `https://<dist-id>.cloudfront.net/avatars/user_...`).
3. Console → S3 → bucket `phim-avatars-<ACCOUNT_ID>` → thư mục `avatars/` → thấy object mới với timestamp trùng lúc upload.

### 12.4. Email qua SES

Trên web: **Đăng ký tài khoản mới** (bằng email đã verify nếu còn sandbox) hoặc dùng **Quên mật khẩu** — mở hộp thư nhận được email do backend gửi qua SES (người gửi = identity đã verify).

### 12.5. Cron chạy đúng

Lambda → `phim-cron` → tab **Test** → Invoke với `{}` → CloudWatch → `/aws/lambda/phim-cron` → log mới nhất có `[Cron] checkExpiredSubscriptions: {...}`.

### 12.6. Alarm → SNS bắn email

Cách ép alarm chuyển trạng thái nhanh (chọn 1):

- **Cách A (khuyến nghị — không phá gì):** tạm sửa alarm `phim-backend-errors` → threshold metric **Invocations ≥ 1** thay vì Errors → curl API 1 lần → alarm chuyển **In alarm** → nhận email → sửa alarm về Errors như cũ.
- **Cách B:** gây lỗi thật — Lambda `phim-backend` → Test với event rác `{"rawPath": null}` vài lần để function throw error.

Kết quả: alarm chuyển **In alarm** (đỏ), hộp thư nhận email `ALARM: "phim-backend-errors" in Asia Pacific (Singapore)`, sau đó alarm quay lại OK.

### 12.7. Tổng hợp metric sau kiểm thử

CloudWatch → Metrics → xem lại Invocations/Duration/Errors của `phim-backend` và Count/4xx/5xx của `phim-api` sau cả buổi test — có dữ liệu thật để phân tích trong báo cáo (VD: cold start đầu tiên ~3–5s, các request sau ~100–300ms).

### ✅ Kết quả mong đợi

Đủ 7 bằng chứng: happy path, 404/401, object S3, email SES, log cron, email alarm, metric dashboard.

### 🛠 Troubleshooting

| Vấn đề | Xử lý |
|---|---|
| Upload avatar lỗi 500 | Xem log `/aws/lambda/phim-backend`: thiếu env `S3_AVATAR_BUCKET`/quyền S3 (bước 6), hoặc thiếu `AVATAR_PUBLIC_BASE_URL` |
| Không nhận được email đăng ký | SES sandbox — người nhận phải verify (bước 5.1) |
| Alarm không bắn email | Subscription chưa Confirmed; hoặc alarm chưa đủ datapoint — chờ hết period 5 phút |
