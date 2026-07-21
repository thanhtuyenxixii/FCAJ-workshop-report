---
title : "CloudFront + ACM + WAF"
date : 2024-01-01
weight : 10
chapter : false
pre : " <b> 5.10 </b> "
---

### Goal

Put **CloudFront** in front of everything: one HTTPS domain serving the frontend (Amplify origin), the API (API Gateway origin) and avatars (S3 origin via **OAC** — bucket stays private). Attach a **WAF Web ACL** with managed rules to block layer-7 attacks at the edge.

### 10.1. Note on ACM

With the default `*.cloudfront.net` domain **no certificate work is needed**. ACM matters only for custom domains: create the cert in **us-east-1** for CloudFront/Amplify and `ap-southeast-1` for an API Gateway custom domain. This workshop uses the default domain.

### 10.2. Create the distribution

1. **CloudFront** → **Create distribution**.
2. **Origin 1 — Amplify (default):** **origin domain** `<branch>.<app-id>.amplifyapp.com` (typed manually), **Protocol** HTTPS only.

![distribution options](/images/5-Workshop/5.10-CloudFront-WAF/01-distribution-options.png)
![origin amplify](/images/5-Workshop/5.10-CloudFront-WAF/02-origin-amplify.png)

3. **Default behavior:** Viewer protocol policy **Redirect HTTP to HTTPS**; allowed methods **GET, HEAD, OPTIONS, PUT, POST, PATCH, DELETE**; cache policy **UseOriginCacheControlHeaders**; origin request policy **AllViewerExceptHostHeader**.

![default behavior settings](/images/5-Workshop/5.10-CloudFront-WAF/03-default-behavior-settings.png)
![cache settings](/images/5-Workshop/5.10-CloudFront-WAF/04-cache-settings.png)

4. → **Create distribution** (leave WAF off for now — attached in 10.4). Wait for status **Deployed** (~5 minutes).
5. **Origin 2 — API Gateway:** tab **Origins** → **Create origin**: **origin domain** `<api-id>.execute-api.ap-southeast-1.amazonaws.com`, Protocol HTTPS only.

![origin api gateway](/images/5-Workshop/5.10-CloudFront-WAF/05-origin-apigateway.png)

6. Tab **Behaviors** → **Create behavior**: **Path pattern** `api/*` · origin = the API Gateway origin; viewer protocol Redirect HTTP to HTTPS; allowed methods **all** (GET…DELETE); **Cache policy: CachingDisabled** (APIs must not be cached by default); **Origin request policy: AllViewerExceptHostHeader** (forwards headers/query but drops Host — required for API GW).

![behavior api](/images/5-Workshop/5.10-CloudFront-WAF/06-behavior-api.png)
![check behaviors](/images/5-Workshop/5.10-CloudFront-WAF/07-check-behaviors.png)

7. **Origin 3 — S3 avatars:** tab Origins → Create origin: pick bucket `phim-avatars-<ACCOUNT_ID>.s3.ap-southeast-1.amazonaws.com` from the dropdown → **Origin access control settings (OAC)** → **Create new OAC** (keep defaults, Sign requests) → select the new OAC.

![origin s3 avatar](/images/5-Workshop/5.10-CloudFront-WAF/08-origin-s3-avatar.png)
![origin access control](/images/5-Workshop/5.10-CloudFront-WAF/09-origin-access-control.png)

The console shows a yellow warning "You must update the S3 bucket policy" → click **Copy policy** → open S3 → the bucket → **Permissions → Bucket policy → Edit** → paste → Save.

![edit bucket policy](/images/5-Workshop/5.10-CloudFront-WAF/10-edit-bucket-policy.png)

{{% notice note %}}
This pasted bucket policy is proof that "the bucket is private; only this CloudFront distribution can read it."
{{% /notice %}}

8. Tab Behaviors → Create behavior: **Path pattern `avatars/*`** → origin S3 → cache policy **CachingOptimized** → Create.

![behavior avatars](/images/5-Workshop/5.10-CloudFront-WAF/11-behavior-avatars.png)

The Behaviors tab should now list 3 rows: `api/*` → API GW, `avatars/*` → S3, `Default (*)` → Amplify.

### 10.3. Point avatars at CloudFront

`phim-backend` Lambda → **Configuration → Environment variables → Edit** → add:

| Key | Value |
|---|---|
| `AVATAR_PUBLIC_BASE_URL` | `https://<dist-id>.cloudfront.net` |

→ Save. Avatar URLs returned by the backend now look like `https://<dist-id>.cloudfront.net/avatars/...` — served through the CDN while the bucket stays locked.

![update avatar base url](/images/5-Workshop/5.10-CloudFront-WAF/12-update-avatar-base-url.png)

### 10.4. Attach the WAF Web ACL

1. Console → **WAF & Shield** → top-right scope **Global (CloudFront)** → **Web ACLs** → **Create web ACL**.
2. **Name:** `phim-waf` · Resource type: **CloudFront distributions** → **Add AWS resources** → select the distribution you just created.
3. **Add rules → Add managed rule groups → AWS managed rule groups**, check 2 free groups:
   - **Core rule set** (`AWSManagedRulesCommonRuleSet`) — blocks common SQLi/XSS/LFI
   - **Amazon IP reputation list** (`AWSManagedRulesAmazonIpReputationList`) — blocks known malicious IPs
4. **Default action: Allow** → Next → … → **Create web ACL**.

### 10.5. Verify

```powershell
# FE through CloudFront
curl -I https://<dist-id>.cloudfront.net/
# HTTP/2 200, header x-cache: Hit/Miss from cloudfront

# API through CloudFront
curl https://<dist-id>.cloudfront.net/api/health-check
# {"status":"ok","message":"Server is running"}
```

### Expected result

- One CloudFront domain serves FE + API + avatars; WAF associated; the S3 bucket still blocks all public access.

### 🛠 Troubleshooting

| Error | Fix |
|---|---|
| `403 Forbidden` on avatars | Bucket policy missing/wrong — redo 10.2 step 7 |
| API via CloudFront returns `403 {"message":"Forbidden"}` | Missing `AllViewerExceptHostHeader` origin request policy — API GW rejects the CloudFront Host header |
| Odd FE errors behind CloudFront | Invalidate cache: CloudFront → Invalidations → `/*` |
| Can't find the Global (CloudFront) scope in WAF | Switch the region picker to **Global** when creating the Web ACL |
