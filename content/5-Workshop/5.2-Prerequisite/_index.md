---
title : "Prerequisite"
date : 2024-01-01
weight : 2
chapter : false
pre : " <b> 5.2 </b> "
---

### Goal

Prepare the AWS account, an admin IAM user (never use root), the AWS CLI, and the source code for the following steps.

### 2.1. AWS account & region

1. Sign in to the [AWS Console](https://console.aws.amazon.com/), or register at [aws.amazon.com](https://aws.amazon.com/) (credit card required; this workshop stays mostly within the free tier).
2. In the top-right corner, select **Asia Pacific (Singapore) `ap-southeast-1`** — all later steps (except WAF/ACM for CloudFront) happen in this region.

![region](/images/5-Workshop/5.2-Prerequisite/01-region.png)

### 2.2. Create an admin IAM user (do not use root)

First security principle: **root is only used to create an IAM user, then locked away**.

1. Console → **IAM** → **Users** → **Create user**.
2. User name: `phim-admin` → check **Provide user access to the AWS Management Console** → **I want to create an IAM user** → set a password.

![create iam user](/images/5-Workshop/5.2-Prerequisite/02-create-iam-user.png)

3. Permissions: **Attach policies directly** → check **`AdministratorAccess`** → **Next** → **Create user**.

![attach policy](/images/5-Workshop/5.2-Prerequisite/03-attach-policy.png)

4. Enable MFA: IAM → Users → `phim-admin` → **Security credentials** → **Assign MFA device** → Authenticator app → scan the QR code.

![assign mfa](/images/5-Workshop/5.2-Prerequisite/04-assign-mfa.png)

5. Sign out of root and sign back in as `phim-admin`. All later steps use this user.

### 2.3. Install AWS CLI v2 and create an access key

1. Download AWS CLI v2 for Windows: <https://awscli.amazonaws.com/AWSCLIV2.msi> → install → open a new PowerShell:

```powershell
aws --version
# aws-cli/2.x.x Python/3.x.x Windows/10 exe/AMD64
```

![cli version](/images/5-Workshop/5.2-Prerequisite/05-cli-version.png)

2. Create an access key: IAM → Users → `phim-admin` → **Security credentials** → **Create access key** → use case **Command Line Interface (CLI)** → download the .csv (shown only once).

![access key](/images/5-Workshop/5.2-Prerequisite/06-access-key.png)

3. Configure the CLI:

```powershell
aws configure
# AWS Access Key ID:     <paste access key>
# AWS Secret Access Key: <paste secret key>
# Default region name:   ap-southeast-1
# Default output format: json
```

![aws configure](/images/5-Workshop/5.2-Prerequisite/07-aws-configure.png)

4. Verify:

```powershell
aws sts get-caller-identity
```

It must return your `Account` (12 digits — **note it down**; later steps call it `<ACCOUNT_ID>`) and an `Arn` containing `user/phim-admin`.

![sts get-caller-identity](/images/5-Workshop/5.2-Prerequisite/08-sts-get-caller-identity.png)

### 2.4. Source code & external services

| Item | Requirement |
|---|---|
| Node.js | v20+ (`node --version`) |
| Backend | `cd phim-be && npm ci` succeeds; `src/lambda.js`, `src/lambdaCron.js`, `src/config/ssm.js`, `src/services/storageService.js` exist in the source |
| Frontend | `phim-fe/` pushed to **GitHub** (Amplify will connect to it); `amplify.yml` present |
| MongoDB Atlas | Cluster + `mongodb+srv://...` connection string; Network Access allows `0.0.0.0/0` (protected by TLS + a strong password) |
| Upstash Redis | A `rediss://...` REDIS_URL (free tier) |
| Test email | 1–2 addresses you can access (for SES verification and SNS alerts) |

### ✅ Expected result

- Console signed in as `phim-admin` (with MFA), Singapore region.
- `aws sts get-caller-identity` returns the correct account.
- `npm ci` succeeds in `phim-be/`; `phim-fe` repo is on GitHub.

### 🛠 Troubleshooting

| Error | Fix |
|---|---|
| `aws` not recognized | Open a **new** PowerShell after installing; check PATH |
| `InvalidClientTokenId` | Wrong/deleted access key — create a new one and re-run `aws configure` |
| `npm ci` fails on node-gyp/sharp | Ensure Node v20+; delete `node_modules` and retry |
