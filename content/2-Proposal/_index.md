---
title: "Proposal"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# Movie Streaming Platform on AWS
## A Serverless Netflix-clone with HLS Streaming, Monitoring and Security Best Practices

### 1. Executive Summary
This project builds a complete **online movie streaming platform** (HLS streaming), consisting of two independent applications: a **Backend** — a REST API built with Node.js + Express covering \~25 feature groups (movie management, users, JWT authentication, ratings, comments, favorites, watch history, watch progress, Premium subscriptions, advertisements, automated movie crawling, search, and an admin dashboard) — and a **Frontend** — a Next.js 15 application (Pages Router) supporting vi/en bilingual UI, dark/light themes, offline PWA, a custom HLS player, and a TypeScript-based admin area.

The goal of this workshop is to **deploy the entire system onto AWS Serverless infrastructure** (region `ap-southeast-1`): the Next.js SSR frontend runs on **AWS Amplify**, the Express.js backend runs on **Lambda + API Gateway**, all behind **CloudFront** — demonstrating the ability to apply AWS services to a real, end-to-end use case. Target customers are end users who watch movies for free (ad-supported) or via a paid Premium plan (ad-free), and administrators who operate content and monitor the system through a dashboard.

### 2. Problem Statement
### What's the Problem?
1. **Infrastructure cost under uneven traffic**: streaming traffic fluctuates heavily by hour (evening peaks, daytime lows) — fixed servers (EC2/VPS) waste money off-peak and bottleneck at peak.
2. **HLS playback latency**: video is streamed from an external origin; routing every request to the origin is slow and bandwidth-expensive.
3. **Manual operations without observability**: no centralized logging or alerting — failures are only discovered through user complaints.
4. **Security risks**: public APIs are exposed to injection attacks, layer-7 DDoS, and bots; credentials scattered in code.
5. **Scheduled tasks in a serverless environment**: traditional `node-cron` cannot run on serverless (no long-lived process).

### The Solution
A **serverless, pay-per-request, auto-scaling** architecture: Express.js runs on **Lambda + API Gateway (HTTP API)**, Next.js SSR is hosted on **AWS Amplify**, and **CloudFront** caches HLS `.m3u8`/`.ts` segments and static assets close to users. **CloudWatch Logs/Metrics + Alarms + SNS** provide observability and admin alerting; **WAF, IAM least-privilege, and environment variables / Secrets Manager** cover security; a dedicated **EventBridge Scheduler + Lambda** handles daily cron jobs (expired-subscription checks with SES bulk email).

### Benefits and Return on Investment
The platform scales automatically with demand and costs **\~$52–55/month** (further reducible to \~$20/month by dropping the NAT Gateway IP-whitelist requirement). Centralized monitoring replaces reactive, complaint-driven operations, and the bilingual step-by-step workshop produced at the end serves as a reusable learning resource for deploying real Express/Next.js systems on AWS Serverless.

### 3. Solution Architecture
Requests flow through CloudFront (with a WAF Web ACL attached at the edge) to three origins: Amplify (Next.js SSR frontend), API Gateway → Lambda (Express.js backend in a VPC private subnet), and the external HLS video server. The backend Lambda talks to MongoDB Atlas (TLS + IP whitelist) and Upstash Redis (search cache) through a NAT Gateway, to S3 for avatar storage via Pre-signed URLs, and to SES for email. EventBridge Scheduler triggers a daily cron Lambda that checks expired subscriptions. The architecture is detailed below:

![Movie Streaming Platform on AWS Architecture](/images/2-Proposal/awswebxemphimchua.drawio.png)

> 🔒 **Note on WAF:** WAF is not a standalone hop on the network path — the Web ACL is **attached directly to the CloudFront distribution** and evaluates requests at the edge location before CloudFront processes them. A Web ACL used with CloudFront must be created in the Global scope (us-east-1).

### AWS Services Used
- **AWS Lambda**: Runs the Express.js backend + cron jobs — serverless, 1M free requests/month, cold-start optimized code.
- **Amazon API Gateway (HTTP API)**: REST API front door — \~70% cheaper than REST API Gateway, native Lambda integration.
- **AWS Amplify**: Next.js SSR hosting with Git-based CI/CD.
- **Amazon CloudFront**: CDN for HLS video + static assets — cuts latency and origin bandwidth cost.
- **Amazon S3**: User avatar storage with Pre-signed URL direct uploads; all public access blocked.
- **Amazon SES**: Transactional email — $0.10/1,000 emails, replaces Nodemailer/SMTP.
- **Amazon CloudWatch**: Logs + metrics + alarms, built-in integration with Lambda/API Gateway.
- **Amazon SNS**: Admin alerting via direct email subscription.
- **Amazon EventBridge Scheduler**: Cron trigger replacing node-cron in serverless environments.
- **IAM + ACM + WAF**: Least-privilege roles, free SSL/TLS, layer-7 attack protection.
- **VPC + NAT Gateway**: Private networking for Lambda — fixed outbound IP for MongoDB Atlas/Upstash Redis whitelisting.

**Non-AWS services**: MongoDB Atlas M2 (managed DBaaS, 3-node replica set, auto-failover), Upstash Redis (serverless cache, free tier), External Video Server (HLS origin).

### Component Design
- **Content delivery**: CloudFront caches HLS segments and static assets at global edge locations; WAF Web ACL filters requests at the edge.
- **Frontend**: AWS Amplify hosts the Next.js 15 SSR app, deployed automatically from Git.
- **Backend**: Express.js wrapped for Lambda behind API Gateway HTTP API, running in a VPC private subnet.
- **Data layer**: MongoDB Atlas as primary data store; Upstash Redis as search cache — both reached via NAT Gateway with TLS + IP whitelist.
- **Media storage**: S3 bucket for avatars, accessed only via Pre-signed URLs (`BlockPublicAcls=true`).
- **Email**: SES for verification/notification emails and bulk subscription-expiry notices.
- **Cron**: EventBridge Scheduler triggers a dedicated Lambda daily to check expired subscriptions.
- **Observability**: CloudWatch collects logs/metrics; Alarms publish to SNS, which emails the admin on threshold breach.
- **Security**: Lambda Execution Role limited to `s3:GetObject`/`s3:PutObject` on the specific bucket; HTTPS only with ACM certificates; no hard-coded credentials.

### 4. Technical Implementation
**Implementation Phases**
- **Research & design**: learn AWS fundamentals (IAM, Lambda, S3, API Gateway); study serverless patterns for Express/Next.js (Weeks 1–2).
- **Application development**: complete BE/FE features (subscriptions, ads, movie crawler, admin dashboard); optimize cold-start for serverless (Weeks 3–5).
- **AWS infrastructure deployment**: create IAM roles; deploy BE to Lambda + API Gateway; deploy FE to Amplify; configure S3, SES, CloudFront, WAF, VPC/NAT (Weeks 6–8).
- **Monitoring & optimization**: configure CloudWatch Logs/Metrics, Alarm → SNS, EventBridge cron; measure performance and optimize cost (Weeks 9–10).
- **Testing & documentation**: end-to-end and failure testing; write the bilingual step-by-step workshop and clean-up guide (Weeks 11–12).

**Technical Requirements**
- **Backend**: Node.js + Express adapted for Lambda (lazy-required heavy modules, MongoDB connection cached on `global`), deployed behind API Gateway HTTP API inside a VPC private subnet.
- **Frontend**: Next.js 15 (Pages Router, TypeScript admin area) built and hosted on Amplify with SSR support.
- **Networking & security**: VPC with NAT Gateway Elastic IP for database whitelisting; WAF Web ACL in Global scope (us-east-1) attached to CloudFront; ACM certificates for CloudFront, Amplify, and API Gateway; environment variables / Secrets Manager for credentials.

### 5. Timeline & Milestones
**Project Timeline (12 weeks)**
- **Phase 1 — Research & design (Weeks 1–2)**: AWS fundamentals, serverless architecture study → deliverable: this proposal + draw.io architecture diagram.
- **Phase 2 — Application development (Weeks 3–5)**: complete BE/FE features, cold-start optimization → deliverable: end-to-end app running locally.
- **Phase 3 — AWS infrastructure deployment (Weeks 6–8)**: IAM, Lambda + API Gateway, Amplify, S3, SES, CloudFront, WAF, VPC/NAT → deliverable: system live on AWS, publicly reachable.
- **Phase 4 — Monitoring & optimization (Weeks 9–10)**: CloudWatch, Alarm → SNS, EventBridge cron, cost optimization → deliverable: working monitoring dashboard + alerts.
- **Phase 5 — Testing & documentation (Weeks 11–12)**: end-to-end testing, bilingual workshop, clean-up guide → deliverable: workshop website + final report.

### 6. Budget Estimation
### Infrastructure Costs
- AWS Services:
    - AWS Lambda: \~$0/month (free tier 1M requests/month).
    - API Gateway (HTTP API): \~$1/month ($1/million requests).
    - AWS Amplify: \~$1/month (build minutes + SSR hosting).
    - Amazon S3: \~$0.02/month (avatars only, small footprint).
    - Amazon SES: \~$0.10 per 1,000 emails.
    - AWS WAF: \~$6/month (Web ACL + rules).
    - NAT Gateway: \~$32/month (largest cost — required for IP whitelisting).
    - CloudFront CDN: \~$1–2/month (HLS video caching).
    - CloudWatch + SNS: \~$0–1/month (within free tier at this scale).
- Non-AWS services:
    - Upstash Redis: \~$0/month (free tier).
    - MongoDB Atlas M2: \~$9/month (managed DBaaS, 3-node replica set).

**Total: \~$52–55/month**

> 💡 **Optimization path:** The NAT Gateway accounts for \~60% of the cost. Dropping the IP-whitelist requirement (relying on TLS + strong credentials instead) would let Lambda run outside the VPC and cut the total to **\~$20/month**.

### 7. Risk Assessment
#### Risk Matrix
- **Lambda cold-start** slows first requests (large Express app): high severity.
- **NAT Gateway cost** exceeds estimates as traffic grows: medium severity.
- **SES sandbox** only allows sending to verified addresses: medium severity.
- **Socket.io cannot run on Lambda** (no persistent connections): medium severity.
- **External video source (Ophim)** changes structure or shuts down: medium severity.
- **Exceeding free tier** causes unexpected charges: low severity.
- **Credential leakage** via public repos: low severity.

#### Mitigation Strategies
- Cold-start: heavy modules lazy-required (socket.io, swagger, cron); MongoDB connection cached on `global`; consider Provisioned Concurrency for critical endpoints.
- NAT cost: CloudWatch Billing Alarm; plan B — drop VPC, authenticate DB with credentials + TLS.
- SES: request production access early (week 6); fall back to Nodemailer/Gmail SMTP meanwhile.
- Realtime: enabled only on local/VPS; future work — migrate to API Gateway WebSocket.
- Video source: crawler isolated as a swappable service; CloudFront caching reduces origin dependency.
- Cost control: AWS Budgets + Billing Alarm at $60; clean up resources after demo.
- Credentials: `.env` git-ignored; IAM key rotation; secret scanning before commits.

#### Contingency Plans
- If VPC/NAT costs become unsustainable, move Lambda outside the VPC and rely on TLS + strong credentials for database access.
- If SES production access is delayed, temporarily send email via Nodemailer/Gmail SMTP.
- Clean-up guide ensures all resources can be torn down quickly after the demo to stop all charges.

### 8. Expected Outcomes
#### Technical Improvements
The entire platform runs serverless on AWS — auto-scaling with traffic, HTTPS-only, WAF-protected, with centralized CloudWatch monitoring and SNS alerting replacing manual, reactive operations. HLS playback latency is reduced by CloudFront edge caching.

#### Long-term Value
A production-grade reference architecture for deploying Express/Next.js systems on AWS Serverless, a bilingual step-by-step workshop reusable by other learners, and a cost model (\~$52–55/month, optimizable to \~$20/month) validated against real traffic.
