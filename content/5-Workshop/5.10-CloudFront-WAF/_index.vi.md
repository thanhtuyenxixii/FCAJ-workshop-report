---
title : "CloudFront + ACM + WAF"
date : 2024-01-01
weight : 10
chapter : false
pre : " <b> 5.10 </b> "
---

### Mục tiêu

Đến đây chúng ta đã có FE và BE chạy độc lập ở hai domain khác nhau — bước này sẽ hợp nhất chúng lại. Chúng ta sẽ đặt **CloudFront** trước toàn hệ thống: một domain HTTPS duy nhất phục vụ frontend (origin Amplify), API (origin API Gateway) và avatar (origin S3 qua **OAC** — bucket vẫn private), đồng thời gắn **WAF Web ACL** với managed rules để chặn tấn công lớp 7 ngay tại edge.

### 10.1. Ghi chú về ACM

Dùng domain mặc định `*.cloudfront.net` thì **không cần tự tạo certificate** — AWS đã có sẵn. ACM chỉ cần khi bạn gắn domain riêng (vd `phim.example.com`): khi đó tạo certificate ở **us-east-1** cho CloudFront/Amplify và `ap-southeast-1` cho API Gateway custom domain. Workshop này dùng domain mặc định.

### 10.2. Tạo CloudFront distribution

1. Console → **CloudFront** → **Create distribution**.
2. **Origin 1 — Amplify (mặc định):** **Origin domain:** `<branch>.<app-id>.amplifyapp.com` (gõ tay), **Protocol:** HTTPS only.

![distribution options](/images/5-Workshop/5.10-CloudFront-WAF/01-distribution-options.png)
![origin amplify](/images/5-Workshop/5.10-CloudFront-WAF/02-origin-amplify.png)

3. **Default behavior:** Viewer protocol policy: **Redirect HTTP to HTTPS**; Allowed HTTP methods: **GET, HEAD, OPTIONS, PUT, POST, PATCH, DELETE**; Cache policy: **UseOriginCacheControlHeaders** (tôn trọng cache header của Next.js); Origin request policy: **AllViewerExceptHostHeader**.

![default behavior settings](/images/5-Workshop/5.10-CloudFront-WAF/03-default-behavior-settings.png)
![cache settings](/images/5-Workshop/5.10-CloudFront-WAF/04-cache-settings.png)

4. → **Create distribution** (WAF chọn "Do not enable" tạm — bật ở 10.4). Chờ trạng thái **Deployed** (~5 phút).
5. Thêm **Origin 2 — API Gateway:** tab **Origins** → **Create origin**: **Origin domain:** `<api-id>.execute-api.ap-southeast-1.amazonaws.com` · Protocol: HTTPS only.

![origin api gateway](/images/5-Workshop/5.10-CloudFront-WAF/05-origin-apigateway.png)

6. Tab **Behaviors** → **Create behavior**: **Path pattern:** `api/*` · Origin: origin API Gateway; Viewer protocol policy: Redirect HTTP to HTTPS · Allowed methods: **tất cả** (GET…DELETE); **Cache policy: CachingDisabled** ⬅ API không được cache mặc định; **Origin request policy: AllViewerExceptHostHeader** ⬅ chuyển tiếp header/query nhưng bỏ Host (bắt buộc với API GW).

![behavior api](/images/5-Workshop/5.10-CloudFront-WAF/06-behavior-api.png)
![kiểm tra behaviors](/images/5-Workshop/5.10-CloudFront-WAF/07-check-behaviors.png)

7. Thêm **Origin 3 — S3 avatar:** tab Origins → Create origin: **Origin domain:** chọn bucket `phim-avatars-<ACCOUNT_ID>.s3.ap-southeast-1.amazonaws.com` từ dropdown → **Origin access:** **Origin access control settings (OAC)** → **Create new OAC** (giữ mặc định, Sign requests) → chọn OAC vừa tạo.

![origin s3 avatar](/images/5-Workshop/5.10-CloudFront-WAF/08-origin-s3-avatar.png)
![origin access control](/images/5-Workshop/5.10-CloudFront-WAF/09-origin-access-control.png)

Console hiện cảnh báo vàng "You must update the S3 bucket policy" → bấm **Copy policy** → mở S3 → bucket → **Permissions → Bucket policy → Edit** → dán → Save.

![edit bucket policy](/images/5-Workshop/5.10-CloudFront-WAF/10-edit-bucket-policy.png)

{{% notice note %}}
Bucket policy vừa dán là minh chứng "bucket private, chỉ CloudFront distribution này được đọc".
{{% /notice %}}

8. Tab Behaviors → Create behavior: **Path pattern `avatars/*`** → origin S3 → cache policy **CachingOptimized** → Create.

![behavior avatars](/images/5-Workshop/5.10-CloudFront-WAF/11-behavior-avatars.png)

Tab Behaviors giờ có 3 dòng: `api/*` → API GW, `avatars/*` → S3, `Default (*)` → Amplify.

### 10.3. Trỏ avatar về CloudFront

Lambda `phim-backend` → **Configuration → Environment variables → Edit** → thêm:

| Key | Value |
|---|---|
| `AVATAR_PUBLIC_BASE_URL` | `https://<dist-id>.cloudfront.net` |

→ Save. Từ giờ URL avatar do backend trả về có dạng `https://<dist-id>.cloudfront.net/avatars/...` — đi qua CDN, bucket vẫn khóa.

![cập nhật avatar base url](/images/5-Workshop/5.10-CloudFront-WAF/12-update-avatar-base-url.png)

### 10.4. Gắn WAF Web ACL

1. Console → **WAF & Shield** → góc phải chọn scope **Global (CloudFront)** → **Web ACLs** → **Create web ACL**.
2. **Name:** `phim-waf` · Resource type: **CloudFront distributions** → **Add AWS resources** → chọn distribution vừa tạo.
3. **Add rules → Add managed rule groups → AWS managed rule groups**, tick 2 nhóm (free):
   - **Core rule set** (`AWSManagedRulesCommonRuleSet`) — chặn SQLi/XSS/LFI phổ biến
   - **Amazon IP reputation list** (`AWSManagedRulesAmazonIpReputationList`) — chặn IP độc hại đã biết
4. **Default action: Allow** → Next → … → **Create web ACL**.

### 10.5. Kiểm tra

```powershell
# FE qua CloudFront
curl -I https://<dist-id>.cloudfront.net/
# HTTP/2 200, header x-cache: Hit/Miss from cloudfront

# API qua CloudFront
curl https://<dist-id>.cloudfront.net/api/health-check
# {"status":"ok","message":"Server is running"}
```

### Kết quả mong đợi

- 1 domain CloudFront phục vụ cả FE + API + avatar; WAF Associated; bucket S3 vẫn Block Public Access.

### 🛠 Troubleshooting

| Lỗi | Xử lý |
|---|---|
| `403 Forbidden` khi mở avatar | Bucket policy chưa dán/dán sai — làm lại 10.2 mục 7 |
| API qua CloudFront trả `403 {"message":"Forbidden"}` | Thiếu Origin request policy `AllViewerExceptHostHeader` — API GW từ chối khi nhận Host của CloudFront |
| FE lỗi lạ sau khi qua CloudFront | Xóa cache: CloudFront → Invalidations → Create → `/*` |
| Không tìm thấy scope Global (CloudFront) trong WAF | Đổi region ở góc phải sang **Global** khi tạo Web ACL |
