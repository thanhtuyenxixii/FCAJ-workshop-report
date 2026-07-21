---
title : "Monitoring & alerting with CloudWatch + SNS"
date : 2024-01-01
weight : 11
chapter : false
pre : " <b> 5.11 </b> "
---

### Goal

Read the automatic Lambda + API Gateway logs/metrics in **CloudWatch**, create an **SNS topic** with an email subscription, and an **Alarm** that emails the admin when the backend errors.

### 11.1. Logs

1. Console → **CloudWatch** → **Log groups** → open **`/aws/lambda/phim-backend`**.
2. Open the newest stream — per-request logs: `🔐 SSM parameters loaded from /phim/prod`, plus `START / END / REPORT` lines (REPORT includes Duration and Memory Used — great numbers for the report).

![cloudwatch logs](/images/5-Workshop/5.11-Monitoring/01-cloudwatch-logs.png)

### 11.2. Metrics

1. **Metrics → All metrics** → **Lambda → By Function Name** → `phim-backend`: check **Invocations**, **Errors**, **Duration**.
2. **ApiGateway → By Api Id** → `phim-api`: check **Count**, **4xx**, **5xx**, **Latency**.

### 11.3. SNS topic

1. **SNS** → **Topics** → **Create topic**: Standard, name `phim-alerts`.
2. **Create subscription**: Protocol **Email**, endpoint = your admin email.
3. Click **Confirm subscription** in the email.

### 11.4. CloudWatch alarm

1. **Alarms → Create alarm** → metric Lambda → `phim-backend` → **Errors**.
2. Statistic **Sum**, period **5 minutes**, static threshold **>= 1**.
3. Action: In alarm → SNS topic `phim-alerts`.
4. Name: `phim-backend-errors` → Create.

{{% notice tip %}}
(Optional) Add a **Billing alarm** (us-east-1): EstimatedCharges ≥ $25 → same topic `phim-alerts`.
{{% /notice %}}

### ✅ Expected result

- Per-request logs readable; metrics populated; subscription Confirmed; alarm OK.

### 🛠 Troubleshooting

| Error | Fix |
|---|---|
| No log group | The function was never invoked — curl the API once |
| No confirmation email | Check Spam; resend from the SNS console |
| Alarm stuck at `Insufficient data` | Normal with no errors yet; treat missing data as notBreaching to keep it OK |
