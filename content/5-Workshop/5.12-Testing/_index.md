---
title : "Testing & validation"
date : 2024-01-01
weight : 12
chapter : false
pre : " <b> 5.12 </b> "
---

### Goal

Prove the system works end-to-end: successful requests, correct error handling, avatars in S3, SES email, cron runs, alarm emails. Each item below is one piece of evidence for the "Testing & measurement" section of the report.

### 12.1. API happy path

```powershell
curl https://<dist-id>.cloudfront.net/api/health-check
curl "https://<dist-id>.cloudfront.net/api/movies?limit=2"
```

Both calls should return JSON.

### 12.2. Error handling

```powershell
curl -i https://<dist-id>.cloudfront.net/api/khong-ton-tai   # expect 404
curl -i https://<dist-id>.cloudfront.net/api/favorites        # expect 401 without a token
```

The backend should fail gracefully with structured JSON errors, not crash.

### 12.3. Avatar upload → S3

1. On the site: sign in → **Profile → change avatar** → upload an image.
2. The new avatar renders (URL like `https://<dist-id>.cloudfront.net/avatars/user_...`).
3. S3 → `phim-avatars-<ACCOUNT_ID>` → `avatars/` → the new object is there with a matching timestamp.

### 12.4. Email via SES

**Register a new account** (with a verified address while in sandbox) or use **Forgot password** — the email arrives via SES with the verified identity as sender.

### 12.5. Cron works

Invoke `phim-cron` with `{}` from the **Test** tab → the newest `/aws/lambda/phim-cron` log shows `[Cron] checkExpiredSubscriptions: {...}`.

### 12.6. Alarm → SNS email

Force the alarm quickly (pick one):

- **Option A (recommended — harmless):** temporarily switch the alarm metric to **Invocations ≥ 1** → curl once → **In alarm** → email received → switch back to Errors.
- **Option B:** cause a real error — Test `phim-backend` with a garbage event `{"rawPath": null}` a few times.

Result: the alarm turns **In alarm** (red), and the SNS email `ALARM: "phim-backend-errors" in Asia Pacific (Singapore)` arrives; it returns to OK afterward.

### 12.7. Metrics wrap-up

Review Invocations/Duration/Errors of `phim-backend` and Count/4xx/5xx of `phim-api` after the session — real data to analyze in the report (e.g. first cold start ~3–5s, warm requests ~100–300ms).

### Expected result

All 7 pieces of evidence: happy path, 404/401, S3 object, SES email, cron log, alarm email, metrics dashboard.

### 🛠 Troubleshooting

| Issue | Fix |
|---|---|
| Avatar upload 500 | Check `/aws/lambda/phim-backend` logs: missing `S3_AVATAR_BUCKET`/S3 permission (step 6), or missing `AVATAR_PUBLIC_BASE_URL` |
| No registration email | SES sandbox — the recipient must be verified (step 5.1) |
| Alarm never emails | Subscription not Confirmed, or not enough datapoints — wait out the 5-minute period |
