---
title : "Configure email with Amazon SES"
date : 2024-01-01
weight : 5
chapter : false
pre : " <b> 5.5 </b> "
---

### Goal

Let the backend send email (verification, password reset, notifications) through **Amazon SES**. We use the SES **SMTP interface**, so **no code changes** — `src/config/email.js` already supports `SMTP_HOST`/`SMTP_PORT`/`EMAIL_USER`/`EMAIL_PASS`.

### 5.1. Verify the sender address

1. Console → **Amazon SES** (`ap-southeast-1`) → **Identities** → **Create identity**.
2. Choose **Email address** → enter your sender address → **Create identity**.
3. Open your inbox → click the confirmation link.
4. Back in SES the status becomes **Verified**.

![create ses identity](/images/5-Workshop/5.5-SES-Email/01-create-ses-identity.png)

{{% notice warning %}}
**SES Sandbox:** new accounts can only send **to** verified addresses. Verify 1–2 extra recipient addresses for testing.
{{% /notice %}}

### 5.2. Create SMTP credentials

1. SES → **SMTP settings** → note the **SMTP endpoint** `email-smtp.ap-southeast-1.amazonaws.com`, port `587` (STARTTLS).
2. Click **Create SMTP credentials** → AWS creates a dedicated IAM user (`ses-smtp-user.xxx`) → **Create user**.
3. **Download/copy immediately** the SMTP user name and password (shown once).

### 5.3. Store SMTP credentials in SSM

Back in **Systems Manager → Parameter Store**, create/edit 4 SecureString parameters: `/phim/prod/SMTP_HOST`, `/phim/prod/SMTP_PORT` (=`587`), `/phim/prod/EMAIL_USER`, `/phim/prod/EMAIL_PASS`.

### 5.4. (Recommended) Request production access

SES → **Account dashboard** → **Request production access** → Mail type `Transactional`, your FE URL, use-case description, bounce/complaint handling. Usually approved within ~24h.

### Quick end-to-end check

Send yourself a test email through the app (e.g. registration or password reset) to confirm the SMTP credentials work end-to-end.

![send test email](/images/5-Workshop/5.5-SES-Email/02-send-test-email.png)
![receive test email](/images/5-Workshop/5.5-SES-Email/03-receive-test-email.png)

### ✅ Expected result

- At least one **Verified** identity; SMTP credentials created; the 4 SMTP parameters stored in SSM.

### 🛠 Troubleshooting

| Error | Fix |
|---|---|
| No verification email | Check Spam; resend from SES |
| `554 Message rejected: Email address is not verified` | Sandbox mode and the **recipient** isn't verified — verify them or request production access |
| `535 Authentication Credentials Invalid` | SMTP password ≠ IAM secret key — use the pair from "Create SMTP credentials" |
