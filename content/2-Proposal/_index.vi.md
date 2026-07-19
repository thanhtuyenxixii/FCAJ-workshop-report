---
title: "Bản đề xuất"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# Movie Streaming Platform on AWS
## Nền tảng xem phim trực tuyến Serverless với HLS Streaming, giám sát và bảo mật

### 1. Tóm tắt điều hành
Dự án xây dựng **nền tảng xem phim trực tuyến** hoàn chỉnh (streaming qua giao thức HLS), gồm hai ứng dụng độc lập: **Backend** — REST API viết bằng Node.js + Express với \~25 nhóm chức năng (quản lý phim, người dùng, xác thực JWT, đánh giá, bình luận, danh sách yêu thích, lịch sử xem, tiến độ xem, gói Premium/subscription, quảng cáo, crawl phim tự động, tìm kiếm và admin dashboard) — và **Frontend** — ứng dụng Next.js 15 (Pages Router), hỗ trợ song ngữ vi/en, dark/light theme, PWA offline, player HLS tùy biến và khu vực quản trị viết bằng TypeScript.

Mục tiêu của workshop là **triển khai toàn bộ hệ thống lên hạ tầng AWS Serverless** (region `ap-southeast-1`): frontend Next.js SSR chạy trên **AWS Amplify**, backend Express.js chạy trên **Lambda + API Gateway**, tất cả phía sau **CloudFront** — thể hiện năng lực vận dụng các dịch vụ AWS vào một use-case thực tế, end-to-end. Khách hàng mục tiêu là người dùng cuối xem phim miễn phí (kèm quảng cáo) hoặc trả phí (Premium, không quảng cáo), và quản trị viên vận hành nội dung, theo dõi hệ thống qua dashboard.

### 2. Tuyên bố vấn đề
*Vấn đề hiện tại*
1. **Chi phí hạ tầng cho traffic không đều**: lượng truy cập dao động mạnh theo giờ (cao điểm buổi tối, thấp điểm ban ngày) — thuê server cố định (EC2/VPS) gây lãng phí lúc thấp điểm và nghẽn lúc cao điểm.
2. **Độ trễ khi phát video HLS**: video được stream từ nguồn ngoài; nếu mọi request đều về origin thì chậm và tốn băng thông.
3. **Vận hành thủ công thiếu giám sát**: không có log tập trung và cảnh báo — lỗi chỉ được phát hiện khi người dùng phàn nàn.
4. **Rủi ro bảo mật**: API công khai dễ bị tấn công (injection, DDoS lớp 7, bot); credentials rải rác trong code.
5. **Tác vụ định kỳ trên serverless**: `node-cron` truyền thống không chạy được trên serverless (không có process thường trực).

*Giải pháp*
Kiến trúc **serverless, trả tiền theo request, tự scale**: Express.js chạy trên **Lambda + API Gateway (HTTP API)**, Next.js SSR host trên **AWS Amplify**, **CloudFront** cache các segment HLS `.m3u8`/`.ts` và static assets gần người dùng. **CloudWatch Logs/Metrics + Alarm + SNS** cung cấp giám sát và cảnh báo admin; **WAF, IAM least-privilege, biến môi trường/Secrets Manager** đảm nhiệm bảo mật; **EventBridge Scheduler + Lambda** riêng xử lý cron job hàng ngày (kiểm tra subscription hết hạn, gửi email hàng loạt qua SES).

*Lợi ích và hoàn vốn đầu tư (ROI)*
Nền tảng tự động scale theo nhu cầu với chi phí **\~$52–55/tháng** (có thể giảm còn \~$20/tháng nếu bỏ yêu cầu IP whitelist qua NAT Gateway). Giám sát tập trung thay thế cách vận hành bị động dựa vào phàn nàn của người dùng, và workshop song ngữ step-by-step tạo ra ở cuối dự án là tài nguyên học tập tái sử dụng cho việc triển khai hệ thống Express/Next.js thực tế lên AWS Serverless.

### 3. Kiến trúc giải pháp
Request đi qua CloudFront (WAF Web ACL gắn tại edge) đến ba origin: Amplify (frontend Next.js SSR), API Gateway → Lambda (backend Express.js trong VPC private subnet), và External Video Server (nguồn HLS). Lambda backend kết nối MongoDB Atlas (TLS + IP whitelist) và Upstash Redis (cache tìm kiếm) qua NAT Gateway, dùng S3 lưu avatar qua Pre-signed URL, và SES gửi email. EventBridge Scheduler trigger Lambda cron hàng ngày kiểm tra subscription hết hạn. Chi tiết kiến trúc:

![Movie Streaming Platform on AWS Architecture](/images/2-Proposal/awswebxemphimchua.drawio.png)

> 🔒 **Lưu ý về WAF:** WAF không phải một "trạm" độc lập trên đường truyền — Web ACL được **gắn trực tiếp vào CloudFront distribution** và đánh giá request ngay tại edge location trước khi CloudFront xử lý. Web ACL cho CloudFront bắt buộc tạo ở scope Global (us-east-1).

*Dịch vụ AWS sử dụng*
- *AWS Lambda*: Chạy backend Express.js + cron jobs — serverless, free tier 1M request/tháng, code đã tối ưu cold-start.
- *Amazon API Gateway (HTTP API)*: Cửa ngõ REST API — rẻ hơn REST API Gateway \~70%, tích hợp Lambda native.
- *AWS Amplify*: Hosting Next.js SSR, CI/CD tự động từ Git.
- *Amazon CloudFront*: CDN cho video HLS + static — giảm độ trễ và chi phí băng thông origin.
- *Amazon S3*: Lưu avatar người dùng, upload trực tiếp qua Pre-signed URL; chặn toàn bộ public access.
- *Amazon SES*: Gửi email hệ thống — $0.10/1.000 email, thay thế Nodemailer/SMTP.
- *Amazon CloudWatch*: Log + metric + alarm, tích hợp sẵn với Lambda/API Gateway.
- *Amazon SNS*: Cảnh báo admin qua email subscription trực tiếp.
- *Amazon EventBridge Scheduler*: Trigger cron, thay thế node-cron trên môi trường serverless.
- *IAM + ACM + WAF*: Least-privilege role, SSL/TLS miễn phí, chặn tấn công lớp 7.
- *VPC + NAT Gateway*: Mạng riêng cho Lambda — cố định IP outbound để whitelist với MongoDB Atlas/Upstash Redis.

**Dịch vụ ngoài AWS**: MongoDB Atlas M2 (managed DBaaS, replica set 3 node, auto-failover), Upstash Redis (cache serverless, free tier), External Video Server (nguồn HLS).

*Thiết kế thành phần*
- *Phân phối nội dung*: CloudFront cache segment HLS và static assets tại edge location toàn cầu; WAF Web ACL lọc request tại edge.
- *Frontend*: AWS Amplify host ứng dụng Next.js 15 SSR, deploy tự động từ Git.
- *Backend*: Express.js đóng gói cho Lambda phía sau API Gateway HTTP API, chạy trong VPC private subnet.
- *Tầng dữ liệu*: MongoDB Atlas là kho dữ liệu chính; Upstash Redis cache tìm kiếm — cả hai truy cập qua NAT Gateway với TLS + IP whitelist.
- *Lưu trữ media*: S3 bucket cho avatar, chỉ truy cập qua Pre-signed URL (`BlockPublicAcls=true`).
- *Email*: SES gửi email xác thực/thông báo và email hàng loạt khi subscription hết hạn.
- *Cron*: EventBridge Scheduler trigger Lambda riêng hàng ngày kiểm tra subscription hết hạn.
- *Giám sát*: CloudWatch thu log/metric; Alarm đẩy sang SNS gửi email cho admin khi vượt ngưỡng.
- *Bảo mật*: Lambda Execution Role chỉ có `s3:GetObject`/`s3:PutObject` trên bucket cụ thể; HTTPS-only với chứng chỉ ACM; không hard-code credentials.

### 4. Triển khai kỹ thuật
*Các giai đoạn triển khai*
1. *Nghiên cứu & thiết kế*: học AWS cơ bản (IAM, Lambda, S3, API Gateway); khảo sát kiến trúc serverless cho Express/Next.js (Tuần 1–2).
2. *Hoàn thiện ứng dụng*: hoàn thiện tính năng BE/FE (subscription, quảng cáo, crawl phim, admin dashboard); tối ưu cold-start cho serverless (Tuần 3–5).
3. *Triển khai hạ tầng AWS*: tạo IAM roles; deploy BE lên Lambda + API Gateway; deploy FE lên Amplify; cấu hình S3, SES, CloudFront, WAF, VPC/NAT (Tuần 6–8).
4. *Giám sát & tối ưu*: cấu hình CloudWatch Logs/Metrics, Alarm → SNS, EventBridge cron; đo hiệu năng, tối ưu chi phí (Tuần 9–10).
5. *Kiểm thử & tài liệu*: test end-to-end, kiểm thử lỗi; viết workshop song ngữ step-by-step, hướng dẫn clean-up (Tuần 11–12).

*Yêu cầu kỹ thuật*
- *Backend*: Node.js + Express điều chỉnh cho Lambda (lazy-require module nặng, cache kết nối MongoDB trên `global`), deploy sau API Gateway HTTP API trong VPC private subnet.
- *Frontend*: Next.js 15 (Pages Router, khu vực admin TypeScript) build và host trên Amplify với hỗ trợ SSR.
- *Mạng & bảo mật*: VPC với NAT Gateway Elastic IP để whitelist database; WAF Web ACL scope Global (us-east-1) gắn vào CloudFront; chứng chỉ ACM cho CloudFront, Amplify và API Gateway; biến môi trường/Secrets Manager cho credentials.

### 5. Lộ trình & Mốc triển khai
*Lộ trình dự án (12 tuần)*
- *Giai đoạn 1 — Nghiên cứu & thiết kế (Tuần 1–2)*: học AWS cơ bản, khảo sát kiến trúc serverless → bàn giao: bản proposal này + sơ đồ kiến trúc draw.io.
- *Giai đoạn 2 — Hoàn thiện ứng dụng (Tuần 3–5)*: hoàn thiện tính năng BE/FE, tối ưu cold-start → bàn giao: ứng dụng chạy end-to-end ở local.
- *Giai đoạn 3 — Triển khai hạ tầng AWS (Tuần 6–8)*: IAM, Lambda + API Gateway, Amplify, S3, SES, CloudFront, WAF, VPC/NAT → bàn giao: hệ thống chạy trên AWS, truy cập công khai.
- *Giai đoạn 4 — Giám sát & tối ưu (Tuần 9–10)*: CloudWatch, Alarm → SNS, EventBridge cron, tối ưu chi phí → bàn giao: dashboard giám sát + alert hoạt động.
- *Giai đoạn 5 — Kiểm thử & tài liệu (Tuần 11–12)*: test end-to-end, workshop song ngữ, hướng dẫn clean-up → bàn giao: workshop website + báo cáo hoàn chỉnh.

### 6. Ước tính ngân sách
*Chi phí hạ tầng*
- Dịch vụ AWS:
    - AWS Lambda: \~$0/tháng (free tier 1M request/tháng).
    - API Gateway (HTTP API): \~$1/tháng ($1/triệu request).
    - AWS Amplify: \~$1/tháng (build minutes + hosting SSR).
    - Amazon S3: \~$0.02/tháng (chỉ lưu avatar, dung lượng nhỏ).
    - Amazon SES: \~$0.10/1.000 email.
    - AWS WAF: \~$6/tháng (Web ACL + rules).
    - NAT Gateway: \~$32/tháng (chi phí lớn nhất — cần cho IP whitelist).
    - CloudFront CDN: \~$1–2/tháng (cache video HLS).
    - CloudWatch + SNS: \~$0–1/tháng (trong free tier với quy mô nhỏ).
- Dịch vụ ngoài AWS:
    - Upstash Redis: \~$0/tháng (free tier).
    - MongoDB Atlas M2: \~$9/tháng (managed DBaaS, replica set 3 node).

**Tổng: \~$52–55/tháng**

> 💡 **Hướng tối ưu:** NAT Gateway chiếm \~60% chi phí. Nếu bỏ yêu cầu IP whitelist (chuyển sang xác thực bằng TLS + credentials mạnh), có thể đưa Lambda ra khỏi VPC và giảm tổng chi phí xuống **\~$20/tháng**.

### 7. Đánh giá rủi ro
*Ma trận rủi ro*
- **Lambda cold-start** làm chậm request đầu tiên (Express app lớn): mức độ cao.
- **Chi phí NAT Gateway** vượt dự kiến khi traffic tăng: mức độ trung bình.
- **SES sandbox** chỉ gửi được đến email đã verify: mức độ trung bình.
- **Socket.io không chạy được trên Lambda** (không có kết nối thường trực): mức độ trung bình.
- **Nguồn video ngoài (Ophim)** thay đổi cấu trúc hoặc ngừng hoạt động: mức độ trung bình.
- **Vượt free tier** gây phát sinh chi phí ngoài ý muốn: mức độ thấp.
- **Lộ credentials** khi làm việc nhóm/public repo: mức độ thấp.

*Chiến lược giảm thiểu*
- Cold-start: đã lazy-require module nặng (socket.io, swagger, cron); cache kết nối MongoDB trên `global`; cân nhắc Provisioned Concurrency cho endpoint quan trọng.
- Chi phí NAT: đặt CloudWatch Billing Alarm; phương án B — bỏ VPC, xác thực DB bằng credentials + TLS.
- SES: xin production access sớm (tuần 6); dự phòng Nodemailer/Gmail SMTP trong lúc chờ.
- Realtime: chỉ bật khi chạy local/VPS; hướng phát triển — chuyển sang API Gateway WebSocket.
- Nguồn video: crawler tách thành service riêng, dễ thay nguồn; CloudFront cache giảm phụ thuộc origin.
- Kiểm soát chi phí: AWS Budgets + Billing Alarm ở mốc $60; clean-up tài nguyên sau khi demo.
- Credentials: `.env` trong `.gitignore`; IAM key xoay vòng; quét secret trước khi commit.

*Kế hoạch dự phòng*
- Nếu chi phí VPC/NAT quá cao, đưa Lambda ra khỏi VPC và dựa vào TLS + credentials mạnh để truy cập database.
- Nếu SES production access bị chậm, tạm gửi email qua Nodemailer/Gmail SMTP.
- Hướng dẫn clean-up đảm bảo có thể gỡ toàn bộ tài nguyên nhanh chóng sau demo để dừng mọi chi phí.

### 8. Kết quả kỳ vọng
*Cải tiến kỹ thuật*: Toàn bộ nền tảng chạy serverless trên AWS — tự scale theo traffic, HTTPS-only, được WAF bảo vệ, giám sát tập trung bằng CloudWatch và cảnh báo SNS thay cho vận hành thủ công, bị động. Độ trễ phát HLS giảm nhờ CloudFront cache tại edge.

*Giá trị dài hạn*: Kiến trúc tham chiếu production-grade cho việc triển khai hệ thống Express/Next.js lên AWS Serverless, workshop song ngữ step-by-step tái sử dụng được cho người học khác, và mô hình chi phí (\~$52–55/tháng, tối ưu được xuống \~$20/tháng) đã kiểm chứng với traffic thực tế.
