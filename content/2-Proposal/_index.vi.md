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
Dự án xây dựng nền tảng xem phim trực tuyến hoàn chỉnh (streaming qua giao thức HLS), gồm hai ứng dụng độc lập: Backend — REST API viết bằng Node.js + Express với \~25 nhóm chức năng (quản lý phim, người dùng, xác thực JWT, đánh giá, bình luận, danh sách yêu thích, lịch sử xem, tiến độ xem, gói Premium/subscription, quảng cáo, crawl phim tự động, tìm kiếm và admin dashboard) — và Frontend — ứng dụng Next.js 15 (Pages Router), hỗ trợ song ngữ vi/en, dark/light theme, PWA offline, player HLS tùy biến và khu vực quản trị viết bằng TypeScript.

Mục tiêu của workshop là triển khai toàn bộ hệ thống lên hạ tầng AWS Serverless (region `ap-southeast-1`): frontend Next.js SSR chạy trên AWS Amplify, backend Express.js chạy trên Lambda + API Gateway, tất cả phía sau CloudFront — thể hiện năng lực vận dụng các dịch vụ AWS vào một use-case thực tế, end-to-end. Khách hàng mục tiêu là người dùng cuối xem phim miễn phí (kèm quảng cáo) hoặc trả phí (Premium, không quảng cáo), và quản trị viên vận hành nội dung, theo dõi hệ thống qua dashboard.

### 2. Tuyên bố vấn đề
*Vấn đề hiện tại*
1. Chi phí hạ tầng cho traffic không đều: lượng truy cập dao động mạnh theo giờ (cao điểm buổi tối, thấp điểm ban ngày) — thuê server cố định (EC2/VPS) gây lãng phí lúc thấp điểm và nghẽn lúc cao điểm.
2. Băng thông khi phát video HLS: video được stream từ nguồn ngoài; nếu mọi request video đều đi qua backend thì backend trở thành bottleneck băng thông và chi phí.
3. Vận hành thủ công thiếu giám sát: không có log tập trung và cảnh báo — lỗi chỉ được phát hiện khi người dùng phàn nàn.
4. Rủi ro bảo mật: API công khai dễ bị tấn công (injection, DDoS lớp 7, bot); credentials rải rác trong code.
5. Tác vụ định kỳ trên serverless: `node-cron` truyền thống không chạy được trên serverless (không có process thường trực).

*Giải pháp*
Kiến trúc serverless, trả tiền theo request, tự scale: Express.js chạy trên Lambda + API Gateway (HTTP API), Next.js SSR host trên AWS Amplify, CloudFront (gắn WAF Web ACL tại edge) làm CDN phân phối static assets/SSR và định tuyến API. Video HLS được player HLS.js tải trực tiếp từ External Video Server qua URL path — không đi qua backend, nên backend không phải gánh băng thông video. CloudWatch Logs/Metrics + Alarm + SNS cung cấp giám sát và cảnh báo admin; WAF, IAM least-privilege, SSM Parameter Store đảm nhiệm bảo mật; EventBridge Scheduler + Lambda riêng xử lý cron job hàng ngày (kiểm tra subscription hết hạn, gửi email hàng loạt qua SES).

*Lợi ích và hoàn vốn đầu tư (ROI)*
Nền tảng tự động scale theo nhu cầu với chi phí chỉ \~$18–20/tháng — kiến trúc không dùng VPC/NAT Gateway (Lambda kết nối database trực tiếp qua TLS + credentials mạnh) nên tiết kiệm \~$32/tháng so với phương án IP whitelist qua NAT Gateway. Giám sát tập trung thay thế cách vận hành bị động dựa vào phàn nàn của người dùng, và workshop song ngữ step-by-step tạo ra ở cuối dự án là tài nguyên học tập tái sử dụng cho việc triển khai hệ thống Express/Next.js thực tế lên AWS Serverless.

### 3. Kiến trúc giải pháp
Request từ người dùng đi qua CloudFront (WAF Web ACL gắn tại edge) đến hai origin: Amplify (frontend Next.js SSR, luồng Static/SSR) và API Gateway (HTTP API) → Lambda (backend Express.js, luồng API Calls); Amplify cũng "SSR fetch" ngược về API khi render phía server. Riêng video HLS, player HLS.js tải m3u8/segments trực tiếp từ External Video Server qua URL path. Lambda backend truy vấn MongoDB Atlas (Query/Write qua TLS), cache tìm kiếm bằng Upstash Redis (Cache GET/SET), đọc secrets (JWT/DB/SES) từ SSM Parameter Store, lưu avatar trên S3 qua Pre-signed URL và gửi email qua SES. EventBridge Scheduler trigger Lambda hàng ngày (bulk email, kiểm tra subscription hết hạn). CloudWatch thu log/metric, khi vượt ngưỡng Alarm đẩy sang SNS gửi email cảnh báo admin. Chi tiết kiến trúc:

![Movie Streaming Platform on AWS Architecture](/images/2-Proposal/awswebxemphimchua.drawio.png)

Lưu ý về WAF: WAF không phải một "trạm" độc lập trên đường truyền — Web ACL được gắn trực tiếp vào CloudFront distribution và đánh giá request ngay tại edge location trước khi CloudFront xử lý. Web ACL cho CloudFront bắt buộc tạo ở scope Global (us-east-1).

*Dịch vụ AWS sử dụng*
- *AWS Lambda*: Chạy backend Express.js + cron jobs — serverless, free tier 1M request/tháng, code đã tối ưu cold-start.
- *Amazon API Gateway (HTTP API)*: Cửa ngõ REST API — rẻ hơn REST API Gateway \~70%, tích hợp Lambda native.
- *AWS Amplify*: Hosting Next.js SSR, CI/CD tự động từ Git.
- *Amazon CloudFront*: CDN phân phối static assets/SSR và API — edge location toàn cầu giảm độ trễ, là điểm gắn WAF Web ACL.
- *Amazon S3*: Lưu avatar người dùng, upload trực tiếp qua Pre-signed URL; chặn toàn bộ public access.
- *Amazon SES*: Gửi email hệ thống — $0.10/1.000 email, thay thế Nodemailer/SMTP.
- *Amazon CloudWatch*: Log + metric + alarm, tích hợp sẵn với Lambda/API Gateway.
- *Amazon SNS*: Cảnh báo admin qua email subscription trực tiếp.
- *Amazon EventBridge Scheduler*: Trigger cron, thay thế node-cron trên môi trường serverless.
- *AWS Systems Manager Parameter Store*: Lưu tập trung secrets (JWT/DB/SES) — Lambda đọc lúc runtime, không hard-code credentials.
- *IAM + ACM + WAF*: Least-privilege role; chứng chỉ SSL/TLS miễn phí (ACM us-east-1 cho CloudFront + Amplify, ACM ap-southeast-1 cho API Gateway); chặn tấn công lớp 7.

Dịch vụ ngoài AWS: MongoDB Atlas M2 (managed DBaaS, replica set 3 node, auto-failover), Upstash Redis (cache serverless, free tier), External Video Server (nguồn HLS — player tải trực tiếp).

*Thiết kế thành phần*
- *Phân phối nội dung*: CloudFront phục vụ static assets/SSR và API tại edge location toàn cầu; WAF Web ACL lọc request tại edge. Video HLS do player HLS.js tải trực tiếp từ External Video Server.
- *Frontend*: AWS Amplify host ứng dụng Next.js 15 SSR, deploy tự động từ Git.
- *Backend*: Express.js đóng gói cho Lambda phía sau API Gateway HTTP API.
- *Tầng dữ liệu*: MongoDB Atlas là kho dữ liệu chính; Upstash Redis cache tìm kiếm — Lambda kết nối trực tiếp qua TLS + credentials mạnh (không cần VPC/NAT Gateway).
- *Lưu trữ media*: S3 bucket cho avatar, chỉ truy cập qua Pre-signed URL (`BlockPublicAcls=true`).
- *Email*: SES gửi email xác thực/thông báo và email hàng loạt khi subscription hết hạn.
- *Cron*: EventBridge Scheduler trigger Lambda riêng hàng ngày kiểm tra subscription hết hạn.
- *Giám sát*: CloudWatch thu log/metric; Alarm đẩy sang SNS gửi email cho admin khi vượt ngưỡng.
- *Bảo mật*: Lambda Execution Role chỉ có `s3:GetObject`/`s3:PutObject` trên bucket cụ thể; HTTPS-only với chứng chỉ ACM; secrets lưu trong SSM Parameter Store, không hard-code credentials.

### 4. Triển khai kỹ thuật
*Các giai đoạn triển khai*
1. *Nghiên cứu & thiết kế*: học AWS cơ bản (IAM, Lambda, S3, API Gateway); khảo sát kiến trúc serverless cho Express/Next.js (Tuần 1–2).
2. *Hoàn thiện ứng dụng*: hoàn thiện tính năng BE/FE (subscription, quảng cáo, crawl phim, admin dashboard); tối ưu cold-start cho serverless (Tuần 3–5).
3. *Triển khai hạ tầng AWS*: tạo IAM roles; deploy BE lên Lambda + API Gateway; deploy FE lên Amplify; cấu hình S3, SES, CloudFront, WAF, SSM Parameter Store (Tuần 6–8).
4. *Giám sát & tối ưu*: cấu hình CloudWatch Logs/Metrics, Alarm → SNS, EventBridge cron; đo hiệu năng, tối ưu chi phí (Tuần 9–10).
5. *Kiểm thử & tài liệu*: test end-to-end, kiểm thử lỗi; viết workshop song ngữ step-by-step, hướng dẫn clean-up (Tuần 11–12).

*Yêu cầu kỹ thuật*
- *Backend*: Node.js + Express điều chỉnh cho Lambda (lazy-require module nặng, cache kết nối MongoDB trên `global`), deploy sau API Gateway HTTP API.
- *Frontend*: Next.js 15 (Pages Router, khu vực admin TypeScript) build và host trên Amplify với hỗ trợ SSR.
- *Mạng & bảo mật*: WAF Web ACL scope Global (us-east-1) gắn vào CloudFront; chứng chỉ ACM us-east-1 (CloudFront + Amplify) và ap-southeast-1 (API Gateway); secrets (JWT/DB/SES) quản lý qua SSM Parameter Store; MongoDB Atlas/Upstash Redis kết nối qua TLS + credentials mạnh.

### 5. Lộ trình & Mốc triển khai
*Lộ trình dự án (12 tuần)*
- *Giai đoạn 1 — Nghiên cứu & thiết kế (Tuần 1–2)*: học AWS cơ bản, khảo sát kiến trúc serverless → bàn giao: bản proposal này + sơ đồ kiến trúc draw.io.
- *Giai đoạn 2 — Hoàn thiện ứng dụng (Tuần 3–5)*: hoàn thiện tính năng BE/FE, tối ưu cold-start → bàn giao: ứng dụng chạy end-to-end ở local.
- *Giai đoạn 3 — Triển khai hạ tầng AWS (Tuần 6–8)*: IAM, Lambda + API Gateway, Amplify, S3, SES, CloudFront, WAF, SSM Parameter Store → bàn giao: hệ thống chạy trên AWS, truy cập công khai.
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
    - CloudFront CDN: \~$1–2/tháng (static assets/SSR + API).
    - CloudWatch + SNS: \~$0–1/tháng (trong free tier với quy mô nhỏ).
    - SSM Parameter Store: \~$0/tháng (standard parameters miễn phí).
- Dịch vụ ngoài AWS:
    - Upstash Redis: \~$0/tháng (free tier).
    - MongoDB Atlas M2: \~$9/tháng (managed DBaaS, replica set 3 node).

Tổng: \~$18–20/tháng

Lựa chọn kiến trúc tiết kiệm chi phí: Hệ thống không dùng VPC + NAT Gateway (\~$32/tháng) — thay vì IP whitelist qua Elastic IP của NAT Gateway, Lambda xác thực với MongoDB Atlas/Upstash Redis bằng TLS + credentials mạnh, giúp giảm hơn 60% tổng chi phí vận hành.

### 7. Đánh giá rủi ro
*Ma trận rủi ro*
- Lambda cold-start làm chậm request đầu tiên (Express app lớn): mức độ cao.
- SES sandbox chỉ gửi được đến email đã verify: mức độ trung bình.
- Socket.io không chạy được trên Lambda (không có kết nối thường trực): mức độ trung bình.
- Nguồn video ngoài (Ophim) thay đổi cấu trúc hoặc ngừng hoạt động: mức độ trung bình.
- Vượt free tier gây phát sinh chi phí ngoài ý muốn: mức độ thấp.
- Lộ credentials khi làm việc nhóm/public repo: mức độ thấp.

*Chiến lược giảm thiểu*
- Cold-start: đã lazy-require module nặng (socket.io, swagger, cron); cache kết nối MongoDB trên `global`; cân nhắc Provisioned Concurrency cho endpoint quan trọng.
- SES: xin production access sớm (tuần 6); dự phòng Nodemailer/Gmail SMTP trong lúc chờ.
- Realtime: chỉ bật khi chạy local/VPS; hướng phát triển — chuyển sang API Gateway WebSocket.
- Nguồn video: crawler tách thành service riêng, dễ thay nguồn.
- Kiểm soát chi phí: AWS Budgets + Billing Alarm; clean-up tài nguyên sau khi demo.
- Credentials: secrets lưu trong SSM Parameter Store; `.env` trong `.gitignore`; IAM key xoay vòng; quét secret trước khi commit.

*Kế hoạch dự phòng*
- Nếu SES production access bị chậm, tạm gửi email qua Nodemailer/Gmail SMTP.
- Nếu nguồn video ngoài chậm hoặc quá tải, có thể bổ sung CloudFront distribution riêng làm lớp cache cho HLS segments (hướng phát triển).
- Hướng dẫn clean-up đảm bảo có thể gỡ toàn bộ tài nguyên nhanh chóng sau demo để dừng mọi chi phí.

### 8. Kết quả kỳ vọng
*Cải tiến kỹ thuật*: Toàn bộ nền tảng chạy serverless trên AWS — tự scale theo traffic, HTTPS-only, được WAF bảo vệ, giám sát tập trung bằng CloudWatch và cảnh báo SNS thay cho vận hành thủ công, bị động. Video HLS phát trực tiếp từ nguồn, backend không phải gánh băng thông video.

*Giá trị dài hạn*: Kiến trúc tham chiếu production-grade cho việc triển khai hệ thống Express/Next.js lên AWS Serverless, workshop song ngữ step-by-step tái sử dụng được cho người học khác, và mô hình chi phí tối ưu (\~$18–20/tháng, không cần NAT Gateway) đã kiểm chứng với traffic thực tế.
