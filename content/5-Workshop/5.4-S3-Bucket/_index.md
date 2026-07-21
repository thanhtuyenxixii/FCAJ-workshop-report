---
title : "Create the S3 avatar bucket"
date : 2024-01-01
weight : 4
chapter : false
pre : " <b> 5.4 </b> "
---

### Goal

Create a **strictly private** S3 bucket for user avatars. The backend (`STORAGE_DRIVER=s3` driver in `src/services/storageService.js`) uploads objects under the `avatars/` prefix; users read avatars through CloudFront + OAC (configured in step 10).

### Steps

1. Console → **S3** → **Create bucket**.
2. Fill in:
   - **Bucket name:** `phim-avatars-<ACCOUNT_ID>` (replace with your 12-digit account ID — bucket names are globally unique)
   - **Region:** `ap-southeast-1`
   - **Object Ownership:** keep default (ACLs disabled)
   - **Block Public Access settings:** keep **Block *all* public access = ON** (all 4 boxes checked) ✔
   - **Default encryption:** keep SSE-S3 default
3. → **Create bucket**.

![bucket name](/images/5-Workshop/5.4-S3-Bucket/01-bucket-name.png)

{{% notice warning %}}
The **Block Public Access** section must have all 4 boxes ON — proof the bucket is not public (a security criterion in the grading rubric).
{{% /notice %}}

![block public access](/images/5-Workshop/5.4-S3-Bucket/02-block-public-access.png)

![default encryption](/images/5-Workshop/5.4-S3-Bucket/03-default-encryption.png)

4. Configure CORS: open the bucket → **Permissions** tab → **Cross-origin resource sharing (CORS)** → **Edit** → paste:

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

![cors configuration](/images/5-Workshop/5.4-S3-Bucket/04-cors-configuration.png)

{{% notice tip %}}
After steps 9/10, tighten `AllowedOrigins` to your exact Amplify/CloudFront domains (least privilege).
{{% /notice %}}

### ✅ Expected result

- Bucket `phim-avatars-<ACCOUNT_ID>` exists in `ap-southeast-1`; the **Access** column shows "Bucket and objects not public".
- CORS saved under Permissions.

### 🛠 Troubleshooting

| Error | Fix |
|---|---|
| `Bucket name already exists` | Names are global — add a suffix (e.g. `-workshop`) |
| `AccessDenied` on upload later | The Lambda IAM role lacks `s3:PutObject` on `avatars/*` — see step 6 |
| Images don't load on the web | By design! The bucket is private — read via CloudFront OAC (step 10) or presigned URLs |
