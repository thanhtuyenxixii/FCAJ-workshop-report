---
title : "Create secrets with SSM Parameter Store"
date : 2024-01-01
weight : 3
chapter : false
pre : " <b> 5.3 </b> "
---

### Goal

Move all backend secrets (connection strings, JWT secret, email credentials…) into **SSM Parameter Store** as **SecureStrings** (KMS-encrypted). On Lambda with `USE_SSM=true`, `src/config/ssm.js` loads every parameter under the `/phim/prod` prefix into `process.env` at cold start.

{{% notice note %}}
**Naming rule:** `/phim/prod/<VAR_NAME>` → becomes `process.env.<VAR_NAME>`. E.g. `/phim/prod/MONGODB_URI` → `process.env.MONGODB_URI`.
{{% /notice %}}

### Steps

1. Console → **Systems Manager** → left menu **Parameter Store** → **Create parameter**.
2. Create the first parameter:
   - **Name:** `/phim/prod/MONGODB_URI`
   - **Tier:** Standard (free)
   - **Type:** **SecureString** — keep the default KMS key `alias/aws/ssm`
   - **Value:** paste the Atlas `mongodb+srv://...` connection string
   - → **Create parameter**.

![create parameter mongodb](/images/5-Workshop/5.3-SSM-Parameters/01-create-parameter-mongodb.png)

3. Repeat for the following (all **SecureString**, values from your current `phim-be` `.env`):

| Name | Value |
|---|---|
| `/phim/prod/MONGODB_URI` | MongoDB Atlas connection string |
| `/phim/prod/JWT_SECRET` | JWT signing secret (≥ 32 random chars) |
| `/phim/prod/REDIS_URL` | Upstash Redis `rediss://...` URL |
| `/phim/prod/FRONTEND_URL` | Frontend domain — temporarily `http://localhost:3000`, **update after step 9/10** |
| `/phim/prod/SMTP_HOST` | `email-smtp.ap-southeast-1.amazonaws.com` — *fill in at step 5* |
| `/phim/prod/SMTP_PORT` | `587` — *fill in at step 5* |
| `/phim/prod/EMAIL_USER` | SES SMTP username — *fill in at step 5* |
| `/phim/prod/EMAIL_PASS` | SES SMTP password — *fill in at step 5* |

![create parameter jwt](/images/5-Workshop/5.3-SSM-Parameters/02-create-parameter-jwt.png)
![create parameter frontend url](/images/5-Workshop/5.3-SSM-Parameters/03-create-parameter-frontend-url.png)
![create parameter smtp host](/images/5-Workshop/5.3-SSM-Parameters/04-create-parameter-smtp-host.png)
![create parameter email user](/images/5-Workshop/5.3-SSM-Parameters/05-create-parameter-email-user.png)
![create parameter email pass](/images/5-Workshop/5.3-SSM-Parameters/06-create-parameter-email-pass.png)

{{% notice tip %}}
The 4 SMTP parameters can be created now with a placeholder value (`pending`) and **edited** after finishing step 5 (SES) — or you can skip them until you get there.
{{% /notice %}}

4. Verify with the CLI:

```powershell
aws ssm get-parameters-by-path --path /phim/prod --with-decryption --query "Parameters[].Name"
```

![verify cli](/images/5-Workshop/5.3-SSM-Parameters/07-verify-cli.png)

### ✅ Expected result

- At least `MONGODB_URI`, `JWT_SECRET`, `REDIS_URL`, `FRONTEND_URL` exist under `/phim/prod` as SecureStrings.
- `get-parameters-by-path` lists them all.

### 🛠 Troubleshooting

| Error | Fix |
|---|---|
| `AccessDeniedException` from the CLI | `phim-admin` lacks permission or wrong region — check `aws configure` |
| Duplicate name | Names must be unique; use Edit instead of creating again |
| Lambda can't read them later | Prefix mismatch — the `SSM_PREFIX` env must equal `/phim/prod` (no trailing `/`) |
