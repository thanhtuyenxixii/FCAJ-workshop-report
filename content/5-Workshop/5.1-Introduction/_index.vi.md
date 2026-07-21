---
title : "Giới thiệu"
date : 2024-01-01
weight : 1
chapter : false
pre : " <b> 5.1 </b> "
---

🔗 **Sản phẩm demo trực tiếp:** [https://main.d1ktib0li4t40t.amplifyapp.com/](https://main.d1ktib0li4t40t.amplifyapp.com/)

🎬 **Video demo:** [https://drive.google.com/drive/folders/15MoFtgWw3u2Cjx40EQBNj-LHT5x5Z7Fb?usp=sharing](https://drive.google.com/drive/folders/15MoFtgWw3u2Cjx40EQBNj-LHT5x5Z7Fb?usp=sharing)

### Bối cảnh & bài toán

Trước khi bắt tay vào console, hãy cùng nhìn qua bài toán mà chúng ta sắp giải quyết. Dự án là một **nền tảng xem phim trực tuyến** (Netflix-clone) hoàn chỉnh gồm 2 ứng dụng:

- **Backend `phim-be/`** — REST API Node.js + Express: quản lý phim, người dùng, xác thực JWT, đánh giá/bình luận, yêu thích, lịch sử & tiến độ xem, gói Premium, quảng cáo, crawl phim, tìm kiếm, admin dashboard.
- **Frontend `phim-fe/`** — Next.js 15 (Pages Router): song ngữ vi/en, dark/light theme, PWA, player HLS tùy biến (hls.js), khu vực admin TypeScript.

**Khách hàng:** người xem phim miễn phí (kèm quảng cáo) hoặc Premium (không quảng cáo); quản trị viên vận hành nội dung qua dashboard.

Hệ thống hiện đang chạy trên một PaaS đơn lẻ và gặp 4 vấn đề khi muốn vận hành nghiêm túc:

1. **Chi phí hạ tầng với traffic không đều** — cao điểm buổi tối, thấp điểm ban ngày → cần mô hình serverless trả tiền theo request, tự scale.
2. **Độ trễ khi tải trang & media** — không có CDN đứng trước toàn hệ thống.
3. **Thiếu giám sát** — không có log tập trung, không có cảnh báo; lỗi chỉ biết khi người dùng phàn nàn.
4. **Secrets rải rác trong `.env`** — connection string, JWT secret, SMTP password dạng plain-text, dễ lộ.

### Mục tiêu workshop

Kết thúc workshop này, chúng ta sẽ có một hệ thống chạy thật trên AWS với các đầu ra cụ thể sau — mỗi dòng đều có cách tự kiểm chứng, đừng chỉ tin vào "chạy được là xong":

| Đầu ra | Kiểm chứng |
|---|---|
| Backend chạy trên **Lambda + API Gateway** | `curl /api/health-check` trả 200 qua HTTPS |
| Frontend chạy trên **Amplify** | Truy cập domain `*.amplifyapp.com` |
| Một domain **CloudFront** duy nhất cho FE + API + avatar | Mở `https://<dist>.cloudfront.net` |
| Avatar upload lên **S3** (bucket private, đọc qua OAC) | Thấy object trong bucket sau khi upload |
| Email gửi qua **SES** | Nhận được email đăng ký/quên mật khẩu |
| Cron hàng ngày bằng **EventBridge → Lambda** | Log CloudWatch của lần chạy |
| **Alarm → SNS** báo lỗi về email admin | Nhận email khi alarm chuyển trạng thái |

### Kiến trúc & luồng dữ liệu

Sơ đồ dưới đây là "bản đồ" chúng ta sẽ dùng xuyên suốt workshop — mỗi mũi tên tương ứng với một hoặc vài bước trong các phần tiếp theo:

![Sơ đồ kiến trúc](/images/5-Workshop/architecture/architecture-diagram.png)

- **Users → CloudFront:** mọi request đi qua CloudFront; WAF Web ACL kiểm tra tại edge trước, chặn request độc hại (injection, bot, IP xấu).
- **CloudFront → Amplify:** behavior mặc định phục vụ trang Next.js (static + SSR).
- **CloudFront → API Gateway → Lambda:** behavior `api/*` chuyển request REST tới HTTP API; API Gateway invoke Lambda chạy nguyên app Express (bọc bằng `serverless-http`).
- **Player HLS → External Video Server:** video m3u8/segments được player tải trực tiếp từ nguồn ngoài — hạ tầng AWS không gánh băng thông video.
- **Lambda → MongoDB Atlas / Upstash Redis:** dữ liệu chính và cache tìm kiếm, kết nối TLS + credentials (không cần VPC/NAT).
- **Lambda → S3 / SES / SSM:** upload avatar, gửi email, đọc secrets — tất cả qua IAM Execution Role least-privilege.
- **EventBridge Scheduler → Lambda Cron:** 00:00 hàng ngày (giờ VN) chạy kiểm tra subscription hết hạn.
- **CloudWatch → Alarm → SNS:** log/metric tự động thu; khi Lambda lỗi vượt ngưỡng, alarm bắn email cho admin.

### Dịch vụ sử dụng & lý do lựa chọn

| Dịch vụ | Vai trò | Lý do chọn |
|---|---|---|
| **Lambda** | Chạy Express BE + cron | Serverless, free tier 1M request/tháng; code BE đã tối ưu cold-start |
| **API Gateway (HTTP API)** | Cửa ngõ REST API | Rẻ hơn REST API ~70%, tích hợp Lambda native, hỗ trợ route `ANY /{proxy+}` |
| **Amplify** | Hosting Next.js | Hỗ trợ Next.js native, CI/CD tự động từ GitHub, không phải tự quản server |
| **CloudFront** | CDN | Edge toàn cầu; hợp nhất FE + API + avatar về một domain HTTPS duy nhất |
| **WAF** | Tường lửa lớp 7 | Managed rules có sẵn (SQLi/XSS/bot), gắn thẳng vào CloudFront |
| **ACM** | SSL/TLS | Miễn phí, tự gia hạn (us-east-1 cho CloudFront/Amplify; ap-southeast-1 cho API GW) |
| **S3** | Lưu avatar | Bền 99.999999999%, rẻ; bucket private tuyệt đối, đọc qua CloudFront OAC |
| **SES** | Email | $0.10/1000 email; dùng SMTP interface nên không phải sửa code Nodemailer |
| **SNS** | Cảnh báo | Email subscription cho alarm, cấu hình 1 phút |
| **EventBridge Scheduler** | Cron | Thay node-cron trên serverless; hỗ trợ timezone Asia/Ho_Chi_Minh |
| **CloudWatch** | Giám sát | Tự động thu log/metric của Lambda + API GW, không cần agent |
| **SSM Parameter Store** | Secrets | SecureString mã hóa KMS, tier standard miễn phí, đọc bằng IAM role |
| **IAM** | Phân quyền | Execution Role least-privilege — nguyên tắc quyền tối thiểu |

{{% notice tip %}}
**Vì sao không dùng VPC + NAT Gateway?** NAT tốn ~$32/tháng chỉ để có IP cố định whitelist với MongoDB Atlas. Thay bằng TLS + mật khẩu mạnh + DB user quyền tối thiểu → tiết kiệm ~60% tổng chi phí và Lambda cold-start nhanh hơn.
{{% /notice %}}
