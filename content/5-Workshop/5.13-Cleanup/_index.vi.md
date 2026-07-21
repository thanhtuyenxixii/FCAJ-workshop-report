---
title : "Dọn dẹp tài nguyên"
date : 2024-01-01
weight : 13
chapter : false
pre : " <b> 5.13 </b> "
---

### Mục tiêu

Chúc mừng bạn đã hoàn thành workshop! Bước cuối cùng này cũng quan trọng không kém các bước trước: chúng ta sẽ xóa **toàn bộ** tài nguyên đã tạo để không phát sinh chi phí sau khi demo xong. Xóa theo đúng thứ tự dưới đây (ngược với thứ tự tạo, vì có ràng buộc phụ thuộc giữa các dịch vụ).

{{% notice warning %}}
Chỉ làm bước này **sau khi** đã chụp đủ ảnh cho báo cáo!
{{% /notice %}}

### Thứ tự xóa

**1. WAF Web ACL** — WAF & Shield → scope Global (CloudFront) → Web ACLs → `phim-waf` → tab **Associated AWS resources** → **Disassociate** distribution → quay ra list → chọn `phim-waf` → **Delete** (gõ tên xác nhận).

**2. CloudFront distribution** — CloudFront → chọn distribution → **Disable** → chờ trạng thái Deployed (~5 phút) → **Delete**.

**3. Amplify app** — Amplify → chọn app → **App settings → General settings** → **Delete app** (gõ tên xác nhận).

**4. EventBridge schedule** — EventBridge → Scheduler → Schedules → chọn `phim-daily-check-subs` → **Delete**. (Role tự tạo của Scheduler: IAM → Roles → tìm `Amazon-EventBridge-Scheduler-...` → Delete.)

**5. Lambda functions** — Lambda → chọn `phim-backend` → Actions → **Delete**; lặp lại với `phim-cron`.

**6. API Gateway** — API Gateway → chọn `phim-api` → Actions → **Delete**.

**7. CloudWatch + SNS** —
- CloudWatch → Alarms → chọn `phim-backend-errors` (+ billing alarm nếu có) → Actions → Delete.
- CloudWatch → Log groups → xóa `/aws/lambda/phim-backend`, `/aws/lambda/phim-cron`.
- SNS → Topics → `phim-alerts` → Delete (subscription tự mất theo).

**8. SES** — SES → Identities → chọn email → **Delete identity**. IAM → Users → xóa user `ses-smtp-user.xxx` (SMTP credentials).

**9. S3 bucket** — S3 → chọn bucket `phim-avatars-<ACCOUNT_ID>` → **Empty** (gõ *permanently delete* — bucket phải rỗng mới xóa được, gồm cả `avatars/` và `deploy/`) → sau đó **Delete** (gõ tên bucket).

**10. SSM parameters** — Systems Manager → Parameter Store → tick cả 8 parameter `/phim/prod/*` → **Delete**.

**11. IAM** — IAM → Roles → xóa `phim-lambda-role`. (Giữ user `phim-admin` nếu còn dùng AWS; nếu không: xóa access key trong Security credentials rồi xóa user.)

### Kiểm tra lần cuối

1. **Billing and Cost Management** → **Cost Explorer**: xem chi phí theo ngày sau 24h — các dịch vụ về ~$0.
2. (Nên làm) Billing → **Budgets** → tạo budget $5/tháng để nhận email nếu còn tài nguyên nào bị sót.

### Kết quả mong đợi

Toàn bộ 11 nhóm tài nguyên đã xóa; Cost Explorer không còn phát sinh chi phí mới.

### 🛠 Troubleshooting

| Vấn đề | Xử lý |
|---|---|
| Không xóa được bucket | Bucket chưa Empty — làm Empty trước, kiểm tra cả prefix `deploy/` |
| Không xóa được Web ACL | Chưa Disassociate khỏi distribution |
| Không xóa được distribution | Phải **Disable** và chờ Deployed xong mới Delete được |
| Vẫn thấy chi phí lặt vặt | Kiểm tra region khác có tài nguyên sót không (Console → góc phải); xem Cost Explorer group by Service |
