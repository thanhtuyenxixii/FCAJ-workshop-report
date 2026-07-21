---
title : "Cron hàng ngày với EventBridge Scheduler"
date : 2024-01-01
weight : 8
chapter : false
pre : " <b> 5.8 </b> "
---

### Mục tiêu

Vì `node-cron` không chạy được trên môi trường serverless (Lambda không giữ tiến trình sống liên tục), ở bước này chúng ta sẽ thay nó bằng **EventBridge Scheduler**: 00:00 hàng ngày (giờ Việt Nam) trigger Lambda `phim-cron` chạy `src/lambdaCron.js` — kiểm tra subscription hết hạn và gửi email nhắc.

### 8.1. Tạo Lambda `phim-cron`

Dùng **cùng file zip** đã đóng gói ở bước 7, chỉ khác handler:

1. Console → **Lambda** → **Create function**:
   - **Function name:** `phim-cron` · **Runtime:** Node.js 20.x · **Role:** dùng lại **`phim-lambda-role`**

![basic information](/images/5-Workshop/5.8-EventBridge-Cron/01-basic-information.png)

![custom settings](/images/5-Workshop/5.8-EventBridge-Cron/02-custom-settings.png)

2. Upload cùng file `phim-be-lambda.zip` (hoặc từ S3).

![upload from s3](/images/5-Workshop/5.8-EventBridge-Cron/03-upload-from-s3.png)

3. **Runtime settings → Handler:** `src/lambdaCron.handler`.

![handler lambdacron](/images/5-Workshop/5.8-EventBridge-Cron/04-handler-lambdacron.png)

4. **General configuration:** Memory **512 MB**, Timeout **60 giây**.

![edit general config](/images/5-Workshop/5.8-EventBridge-Cron/05-edit-general-config.png)

5. **Environment variables:** `USE_SSM=true` · `SSM_PREFIX=/phim/prod` · `NODE_ENV=production`.

![edit env vars](/images/5-Workshop/5.8-EventBridge-Cron/06-edit-env-vars.png)

### 8.2. Chạy thử ngay

Tab **Test** → event mặc định `{}` → **Test**.

Kết quả mong đợi: status Succeeded, response dạng `{"ok":true,...}`.

![test run](/images/5-Workshop/5.8-EventBridge-Cron/07-test-run.png)

Mở **Monitor → View CloudWatch logs** thấy dòng `[Cron] checkExpiredSubscriptions: {...}`.

![log events](/images/5-Workshop/5.8-EventBridge-Cron/08-log-events.png)

### 8.3. Tạo lịch chạy hàng ngày

1. Console → **Amazon EventBridge** → menu trái **Scheduler → Schedules** → **Create schedule**.
2. **Schedule name:** `phim-daily-check-subs`.

![schedule name](/images/5-Workshop/5.8-EventBridge-Cron/09-schedule-name.png)

3. **Schedule pattern:** **Recurring schedule** → **Cron-based schedule**:
   - Cron expression: `cron(0 0 * * ? *)` (00:00 mỗi ngày)
   - **Timezone:** `Asia/Ho_Chi_Minh` ⬅ quan trọng!
   - Flexible time window: **Off**

![schedule pattern](/images/5-Workshop/5.8-EventBridge-Cron/10-schedule-pattern.png)

4. **Target:** **AWS Lambda → Invoke** → chọn function `phim-cron` → Payload để trống `{}`.

![target detail](/images/5-Workshop/5.8-EventBridge-Cron/11-target-detail.png)

5. **Permissions:** để Scheduler **tự tạo role mới** (Create new role for this schedule) → **Create schedule**.

![permissions](/images/5-Workshop/5.8-EventBridge-Cron/12-permissions.png)

![tạo schedule](/images/5-Workshop/5.8-EventBridge-Cron/13-create-schedule.png)

### Kết quả mong đợi

- Test tay `phim-cron` chạy Succeeded, log ghi kết quả kiểm tra.
- Schedule `phim-daily-check-subs` trạng thái **Enabled**, lần chạy kế tiếp (Next invocation) là 00:00 ngày mai giờ VN.

### 🛠 Troubleshooting

| Lỗi | Xử lý |
|---|---|
| Test lỗi `MongooseServerSelectionError` | Kiểm tra `/phim/prod/MONGODB_URI` + Atlas Network Access |
| Đến giờ mà không chạy | Kiểm tra timezone của schedule; xem tab Monitor của `phim-cron` có invocation không |
| Muốn xem lịch sử chạy | CloudWatch → Log groups → `/aws/lambda/phim-cron` |
