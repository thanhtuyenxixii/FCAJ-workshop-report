---
title : "Introduction"
date : 2024-01-01
weight : 1
chapter : false
pre : " <b> 5.1 </b> "
---

### Context & problem

The project is a complete **online movie streaming platform** (Netflix-clone) with 2 applications:

- **Backend `phim-be/`** — Node.js + Express REST API: movies, users, JWT auth, ratings/comments, favorites, watch history & progress, Premium subscriptions, ads, movie crawler, search, admin dashboard.
- **Frontend `phim-fe/`** — Next.js 15 (Pages Router): vi/en bilingual, dark/light theme, PWA, custom HLS player (hls.js), TypeScript admin area.

**Customers:** viewers watching for free (with ads) or Premium (ad-free); administrators operating content via the dashboard.

The system currently runs on a single PaaS host and faces 4 problems for serious operation:

1. **Infrastructure cost under uneven traffic** — evening peaks, daytime lows → needs pay-per-request, auto-scaling serverless.
2. **Latency for pages & media** — no CDN in front of the whole system.
3. **No observability** — no centralized logs, no alerts; failures are discovered through user complaints.
4. **Secrets scattered in `.env`** — connection strings, JWT secrets, SMTP passwords in plain text.

### Workshop objectives

| Output | Verification |
|---|---|
| Backend on **Lambda + API Gateway** | `curl /api/health-check` returns 200 over HTTPS |
| Frontend on **Amplify** | Reachable at `*.amplifyapp.com` |
| A single **CloudFront** domain for FE + API + avatars | Open `https://<dist>.cloudfront.net` |
| Avatars uploaded to **S3** (private bucket, read via OAC) | Object visible in the bucket after upload |
| Email via **SES** | Registration/password-reset email received |
| Daily cron via **EventBridge → Lambda** | CloudWatch log of the run |
| **Alarm → SNS** emails the admin on errors | Email received when the alarm fires |

### Architecture & data flow

![Architecture diagram](/images/5-Workshop/architecture/architecture-diagram.png)

- **Users → CloudFront:** every request passes through CloudFront; the WAF Web ACL inspects at the edge first, blocking malicious requests (injection, bots, bad IPs).
- **CloudFront → Amplify:** the default behavior serves the Next.js app (static + SSR).
- **CloudFront → API Gateway → Lambda:** the `api/*` behavior forwards REST requests to the HTTP API, which invokes a Lambda running the whole Express app (wrapped with `serverless-http`).
- **HLS player → External Video Server:** m3u8/segments are fetched directly from the external origin — AWS infra does not carry video bandwidth.
- **Lambda → MongoDB Atlas / Upstash Redis:** primary data and search cache over TLS + credentials (no VPC/NAT needed).
- **Lambda → S3 / SES / SSM:** avatar upload, email, secrets — all via a least-privilege IAM execution role.
- **EventBridge Scheduler → Lambda Cron:** daily at 00:00 (VN time) checks expired subscriptions.
- **CloudWatch → Alarm → SNS:** logs/metrics collected automatically; when Lambda errors cross the threshold, the alarm emails the admin.

### Services & selection rationale

| Service | Role | Why |
|---|---|---|
| **Lambda** | Runs the Express BE + cron | Serverless, 1M free requests/month; BE already cold-start optimized |
| **API Gateway (HTTP API)** | REST front door | ~70% cheaper than REST API, native Lambda integration, `ANY /{proxy+}` route |
| **Amplify** | Next.js hosting | Native Next.js support, automatic CI/CD from GitHub, zero server management |
| **CloudFront** | CDN | Global edge; unifies FE + API + avatars under one HTTPS domain |
| **WAF** | Layer-7 firewall | Ready-made managed rules (SQLi/XSS/bots) attached to CloudFront |
| **ACM** | SSL/TLS | Free, auto-renewing (us-east-1 for CloudFront/Amplify; ap-southeast-1 for API GW) |
| **S3** | Avatar storage | 11-nines durability, cheap; strictly private bucket, read via CloudFront OAC |
| **SES** | Email | $0.10/1000 emails; SMTP interface means zero Nodemailer code changes |
| **SNS** | Alerts | Email subscriptions for alarms, one-minute setup |
| **EventBridge Scheduler** | Cron | Replaces node-cron on serverless; supports Asia/Ho_Chi_Minh timezone |
| **CloudWatch** | Monitoring | Automatic Lambda + API GW logs/metrics, no agents |
| **SSM Parameter Store** | Secrets | KMS-encrypted SecureStrings, free standard tier, read via IAM role |
| **IAM** | Access control | Least-privilege execution role |

{{% notice tip %}}
**Why no VPC + NAT Gateway?** NAT costs ~$32/month just for a fixed IP to whitelist with MongoDB Atlas. Replacing IP-whitelisting with TLS + strong credentials + a least-privilege DB user saves ~60% of total cost and speeds up cold starts.
{{% /notice %}}
