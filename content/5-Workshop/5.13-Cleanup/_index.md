---
title : "Clean up"
date : 2024-01-01
weight : 13
chapter : false
pre : " <b> 5.13 </b> "
---

### Goal

Delete **all** created resources so nothing keeps billing after the workshop. Follow the order below (reverse of creation, due to dependencies).

### Deletion order

**1. WAF Web ACL** — WAF & Shield → Global (CloudFront) scope → `phim-waf` → **Associated AWS resources** → **Disassociate** the distribution → then **Delete** the ACL.

**2. CloudFront distribution** — select the distribution → **Disable** → wait for Deployed (~5 min) → **Delete**.

**3. Amplify app** — **App settings → General settings** → **Delete app**.

**4. EventBridge schedule** — Scheduler → Schedules → **delete** `phim-daily-check-subs`. (Also delete the auto-created `Amazon-EventBridge-Scheduler-...` IAM role.)

**5. Lambda functions** — **delete** `phim-backend` and `phim-cron`.

**6. API Gateway** — **delete** `phim-api`.

**7. CloudWatch + SNS** — delete the `phim-backend-errors` alarm (and billing alarm if created), the two `/aws/lambda/...` log groups, and the `phim-alerts` SNS topic (its subscription is removed automatically).

**8. SES** — **delete the email identity**; delete the `ses-smtp-user.xxx` IAM user (SMTP credentials).

**9. S3 bucket** — **Empty** the bucket `phim-avatars-<ACCOUNT_ID>` (including `avatars/` and `deploy/` prefixes) → then **Delete** it.

**10. SSM parameters** — Systems Manager → Parameter Store → select all 8 `/phim/prod/*` parameters → **Delete**.

**11. IAM** — delete the `phim-lambda-role` role. (Keep `phim-admin` if you still use AWS; otherwise delete its access keys, then the user.)

### Final check

1. **Billing and Cost Management** → **Cost Explorer**: check daily costs after 24h — services should drop to ~$0.
2. (Recommended) Billing → **Budgets** → create a $5/month budget to catch anything left running.

### Expected result

All 11 resource groups deleted; no new charges in Cost Explorer.

### Troubleshooting

| Issue | Fix |
|---|---|
| Bucket won't delete | Not empty — Empty it first, including the `deploy/` prefix |
| Web ACL won't delete | Still associated with the distribution |
| Distribution won't delete | Must be **Disabled** and fully Deployed first |
| Small residual charges | Check other regions for leftovers; group Cost Explorer by Service |
