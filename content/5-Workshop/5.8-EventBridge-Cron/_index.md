---
title : "Daily cron with EventBridge Scheduler"
date : 2024-01-01
weight : 8
chapter : false
pre : " <b> 5.8 </b> "
---

### Goal

Replace `node-cron` (which cannot run on serverless) with **EventBridge Scheduler**: at 00:00 daily (Vietnam time) it triggers the `phim-cron` Lambda running `src/lambdaCron.js` — checking expired subscriptions and sending reminder emails.

### 8.1. Create the `phim-cron` Lambda

Use the **same zip** from step 7 — only the handler differs:

1. **Lambda** → **Create function**: **name** `phim-cron`, **runtime** Node.js 20.x, **role** **`phim-lambda-role`**.

![basic information](/images/5-Workshop/5.8-EventBridge-Cron/01-basic-information.png)

![custom settings](/images/5-Workshop/5.8-EventBridge-Cron/02-custom-settings.png)

2. Upload the same `phim-be-lambda.zip` (or from S3).

![upload from s3](/images/5-Workshop/5.8-EventBridge-Cron/03-upload-from-s3.png)

3. **Runtime settings → Handler:** `src/lambdaCron.handler`.

![handler lambdacron](/images/5-Workshop/5.8-EventBridge-Cron/04-handler-lambdacron.png)

4. **General configuration:** Memory **512 MB**, Timeout **60 s**.

![edit general config](/images/5-Workshop/5.8-EventBridge-Cron/05-edit-general-config.png)

5. **Environment variables:** `USE_SSM=true` · `SSM_PREFIX=/phim/prod` · `NODE_ENV=production`.

![edit env vars](/images/5-Workshop/5.8-EventBridge-Cron/06-edit-env-vars.png)

### 8.2. Test it now

**Test** tab → default `{}` event → **Test**. Expect Succeeded with `{"ok":true,...}`.

![test run](/images/5-Workshop/5.8-EventBridge-Cron/07-test-run.png)

**Monitor → View CloudWatch logs** shows a `[Cron] checkExpiredSubscriptions: {...}` line.

![log events](/images/5-Workshop/5.8-EventBridge-Cron/08-log-events.png)

### 8.3. Create the daily schedule

1. Console → **Amazon EventBridge** → **Scheduler → Schedules** → **Create schedule**.
2. **Schedule name:** `phim-daily-check-subs`.

![schedule name](/images/5-Workshop/5.8-EventBridge-Cron/09-schedule-name.png)

3. **Schedule pattern:** **Recurring schedule** → **Cron-based schedule**:
   - Cron expression: `cron(0 0 * * ? *)` (00:00 every day)
   - **Timezone:** `Asia/Ho_Chi_Minh` ⬅ important!
   - Flexible time window: **Off**

![schedule pattern](/images/5-Workshop/5.8-EventBridge-Cron/10-schedule-pattern.png)

4. **Target:** **AWS Lambda → Invoke** → select function `phim-cron` → leave payload `{}`.

![target detail](/images/5-Workshop/5.8-EventBridge-Cron/11-target-detail.png)

5. **Permissions:** let Scheduler **create a new role** (Create new role for this schedule) → **Create schedule**.

![permissions](/images/5-Workshop/5.8-EventBridge-Cron/12-permissions.png)

![create schedule](/images/5-Workshop/5.8-EventBridge-Cron/13-create-schedule.png)

### Expected result

- Manual test of `phim-cron` succeeds and logs the check result.
- Schedule `phim-daily-check-subs` is **Enabled** with the next invocation at 00:00 VN time.

### 🛠 Troubleshooting

| Error | Fix |
|---|---|
| `MongooseServerSelectionError` on test | Check `/phim/prod/MONGODB_URI` + Atlas Network Access |
| Nothing runs at the scheduled time | Check the schedule's timezone; check `phim-cron` Monitor tab for invocations |
| View run history | CloudWatch → Log groups → `/aws/lambda/phim-cron` |
