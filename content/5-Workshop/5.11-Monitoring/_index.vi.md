---
title : "Giám sát & cảnh báo với CloudWatch + SNS"
date : 2024-01-01
weight : 11
chapter : false
pre : " <b> 5.11 </b> "
---

### Mục tiêu

Hệ thống đã chạy xong, nhưng chúng ta cần biết khi nào nó gặp sự cố mà không phải chờ người dùng báo. Ở bước này, chúng ta sẽ xem log/metric tự động của Lambda + API Gateway trong **CloudWatch**, tạo **SNS topic** nhận email, và một **Alarm** tự bắn email cho admin ngay khi backend lỗi.

### 11.1. Xem logs

1. Console → **CloudWatch** → **Log groups** → mở **`/aws/lambda/phim-backend`**.
2. Mở log stream mới nhất — thấy log của từng request: dòng `🔐 SSM parameters loaded from /phim/prod`, các dòng `START / END / REPORT RequestId...` (REPORT có Duration, Memory Used — số liệu tốt cho báo cáo).

![cloudwatch logs](/images/5-Workshop/5.11-Monitoring/01-cloudwatch-logs.png)

### 11.2. Xem metrics

1. CloudWatch → **Metrics → All metrics** → **Lambda → By Function Name** → chọn `phim-backend`: tick **Invocations**, **Errors**, **Duration**.
2. Tương tự: **ApiGateway → By Api Id** → chọn `phim-api`: tick **Count**, **4xx**, **5xx**, **Latency**.

### 11.3. Tạo SNS topic nhận cảnh báo

1. Console → **Simple Notification Service** → **Topics** → **Create topic**: Type: **Standard** · Name: `phim-alerts` → **Create topic**.
2. Trong topic → **Create subscription**: Protocol **Email** → Endpoint = email admin của bạn → **Create subscription**.
3. Mở hộp thư → click **Confirm subscription** trong email "AWS Notification - Subscription Confirmation".

### 11.4. Tạo CloudWatch Alarm

1. CloudWatch → **Alarms → All alarms** → **Create alarm** → **Select metric** → Lambda → By Function Name → `phim-backend` → metric **Errors** → Select.
2. Cấu hình: Statistic: **Sum** · Period: **5 minutes**; Threshold: **Static** · **Greater/Equal** · giá trị **1** (≥1 lỗi trong 5 phút là báo).
3. **Actions:** In alarm → **Select an existing SNS topic** → `phim-alerts`.
4. **Alarm name:** `phim-backend-errors` → Create alarm.

{{% notice tip %}}
(Tuỳ chọn) Tạo thêm **Billing alarm** đề phòng chi phí: CloudWatch (region **us-east-1**) → Billing → EstimatedCharges ≥ $25 → cùng topic `phim-alerts`.
{{% /notice %}}

### ✅ Kết quả mong đợi

- Đọc được log từng request; metric có dữ liệu; subscription Confirmed; alarm ở trạng thái OK.

### 🛠 Troubleshooting

| Lỗi | Xử lý |
|---|---|
| Không thấy log group | Function chưa được invoke lần nào — curl API một lần |
| Không nhận email confirm | Kiểm tra Spam; Resend từ SNS console |
| Alarm mãi ở `Insufficient data` | Bình thường khi chưa có lỗi & period ngắn; treat missing data = notBreaching nếu muốn luôn OK |
