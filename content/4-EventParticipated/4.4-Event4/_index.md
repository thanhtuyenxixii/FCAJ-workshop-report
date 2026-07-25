---
title: "Event 4"
date: 2026-07-25
weight: 4
chapter: false
pre: " <b> 4.4. </b> "
---

## Report: AWS Agentic AI Buildweek Showcase & Sharing – Multi-Agent Systems & Enterprise Solutions

| Event Information | Details |
| :--- | :--- |
| **Event Name** | AWS Agentic AI Buildweek 2026 Showcase & Sharing |
| **Date & Time** | July 25, 2026 (09:00 - 12:00) |
| **Location** | Online (YouTube Livestream & Subtitle Archive) |
| **Attendance Mode** | Online (Reviewing video recording and team presentation decks) |
| **Role** | Attendee |

### Event Objectives

The **AWS Agentic AI Buildweek Showcase & Sharing** was the wrap-up event for the *Agentic AI Buildweek 2026 Hackathon*, organized by **AWS**, **JI Fund**, and the **FCAJ community**.

The session focused on multi-agent infrastructure architectures and real-world AI agent deployments across F&B, market research, and banking/finance. Additionally, speakers from AWS and competing teams shared practical takeaways on cloud cost management (FinOps), handling low-resource Vietnamese data, and keeping project scope tightly managed during hackathons.

### Speakers & Participating Teams

| No. | Speaker / Team | Role / Award | Topic |
| :-: | :--- | :--- | :--- |
| 1 | **Joseph Marazota** | Head of Technology @ *AWS ASEAN* | *Keynote: Shifting to the AI Agent era, breaking mental models, and the Human-in-the-loop principle* |
| 2 | **Nguyen Gia Hung** | Head of Solution Architect @ *AWS Vietnam* | *AWS Vietnam welcome address & certificate presentation for winning teams* |
| 3 | **One Team** | 1st Prize Winner @ *AWS Track* | *KFC Chatbot Project: Multi-channel ordering agent via Zalo/WhatsApp using AWS Agent Core Memory, TinyFish scraper, and Last Verify mechanism* |
| 4 | **Final Scale (Signal C)** | 2nd Prize Winner @ *AWS Track* | *Multi-Agent Market Intelligence: Competitor & market signal analysis built on Value Creation Canvas, LangFuse & Bedrock Guardrails* |
| 5 | **Team Plan C** | Hackathon Team | *SA Professional AI Native App: AI assistant for requirement analysis, automatic Draw.io architecture generation, cost estimation & Terraform IaC output* |
| 6 | **Team 3KA** | Hackathon Team | *Sheper Project & 24h Hackathon Journey: Stress management, lessons on coding under pressure, and risk management* |
| 7 | **Six Pillars** | Outstanding Team @ *FinTech Track* | *Adaptive Workflow Engine for AML: Automated anti-money laundering investigation assistant for banking, reducing false positives by 90–95%* |

---

### Key Highlights

#### 1. Career Guidance & Mindset Shift from AWS Leadership

Key insights shared by **Mr. Joseph Marazota** (Head of Technology, AWS ASEAN) and **Mr. Nguyen Gia Hung** (Head of Solution Architect, AWS Vietnam):

- **Software Development Velocity Shift:** 20 years ago, banks deployed software once a quarter, then moved to every two weeks with Agile/DevOps. In the AI Agent era, systems can automate continuous releases.
- **Unconstrained Mindset:** Young engineers shouldn't feel limited by legacy paradigms or a lack of experience. Fresh mental models help uncover innovative problem-solving approaches.
- **"Human-in-the-loop" Governance:** Amazon runs over 1 million robots in fulfillment centers, but robots still require human logic and direction. Humans remain the essential controlling node in any AI system.

#### 2. KFC Chatbot Project (One Team - 1st Prize)

**One Team** tackled real-world food ordering friction in the F&B sector with key technical implementations:

- **Real-World Problem:** Drawing lessons from McDonald's AI drive-thru test (where context errors led to accidentally ordering 100 chicken nuggets), the team noted that forcing users to download new apps creates high drop-off rates. Bringing the chatbot directly into familiar messaging apps like Zalo or WhatsApp (focusing on Zalo in Vietnam) is a far more practical path.
- **Technical Architecture:**
  - *Dynamic Menu Scraping:* Lacking official KFC APIs, the team used **TinyFish** to scrape dynamic menu data from KFC's website and store it in AWS databases.
  - *Contextual Personalized Memory:* Implemented **AWS Agent Core Memory** to give each user a dedicated agent memory, recalling previous orders for quick re-ordering.
  - *Cost & Performance:* Response latency reached 3–5 seconds at roughly **$0.006 per order** (~75% cost reduction compared to standard serverless setups).
  - *Order Verification (Last Verify):* Added a final order details confirmation step before payment execution to eliminate AI hallucination risks.

#### 3. Multi-Agent Competitor Market Intelligence (Final Scale - 2nd Prize)

**Final Scale** (a student team from FPT) approached competitor market analysis with a practical mindset:

- **Business Drives Technology:** A complex AI architecture has no value if it fails to solve a business need. The team used a **Value Creation & Delivery Canvas** (adapted from the Business Model Canvas) to focus on delivered output rather than unnecessary financial metrics for a hackathon demo.
- **Multi-Agent Architecture & Security:**
  - *Crawler Subagent:* Used Apify for static sites/large data and TinyFish for deep dynamic web scraping.
  - *Raw Data Filtering & Token Optimization:* Filtered out raw noise using plain code before passing data into LLMs, saving token costs and mitigating prompt injection risks from external web content.
  - *Quality Evaluation via LangFuse:* Evaluated output scores using **LangFuse**. Low scores triggered up to 2 retries; persistent low scores were saved to DynamoDB tagged for human review.
  - *Security:* Integrated **Bedrock Guardrails**, AWS Cognito, WAF, and Amplify.

#### 4. AI Assistant for Solution Architects (Team Plan C)

**Team Plan C** introduced the **SA Professional AI Native App** designed to automate workflow architecture design and cost estimation for Solution Architects:

- **Real-World Problem:** Solution Architects often face urgent client demands for cloud architecture diagrams and cost estimations within tight deadlines (overnight or 2–3 days). Manually drawing diagrams, looking up pricing tiers, and writing Infrastructure-as-Code (IaC) scripts consumes extensive effort.
- **Solution & Workflow:**
  - *Natural Language & Document Ingestion:* Supports free-text input or company policy/specification document uploads.
  - *Automated Draw.io Diagram Generation:* AI parses requirements and generates AWS-compliant architecture diagrams directly inside a Draw.io editor interface, allowing manual drag-and-drop tweaks.
  - *Cost Estimation & IaC Output:* Automatically computes project cost breakdowns and generates reusable Terraform / CloudFormation modules.
  - *Policy & Blacklist Enforcement:* Built validation filters to prevent AI from deploying unapproved services (e.g., blocking high-level abstraction services like App Runner in favor of enterprise-managed ECS/Lambda).
  - *Auto-Deployment:* Clicking confirm triggers automated IaC deployment scripts to provision live resources on AWS.

#### 5. Real-Time Crowd Monitoring & YOLO FinOps Lessons (Team 3KA)

**Team 3KA** (a 5-member student team from FPT) presented the **Sheper** project (An AI camera system for detecting and managing crowd congestion at airports, supermarkets, and event venues) along with key 24h hackathon lessons:

- **Sheper Technical Architecture:**
  - *Video Streaming Pipeline:* Ingested CCTV footage directly via **Kinesis Video Streams** into an ECS Fargate cluster.
  - *Object Detection & Tracking:* Combined **YOLO** with **ByteTrack** for real-time person detection, movement ID tracking, and congestion zone visualization.
  - *Agent & Operator Co-pilot:* Utilized Amazon Bedrock linked to DynamoDB/S3 for autonomous monitoring and operator guidance.
- **FinOps Lessons from SageMaker Billing Incident ($48):**
  - In Q&A, the team shared a practical lesson: they initially hosted a large AI model on **Amazon SageMaker** for a 3-hour demo run, which unexpectedly cost **$48**.
  - They swiftly pivoted to **YOLOv26 Small**, maintaining high confidence scores (90%–97%) while heavily cutting cloud infrastructure costs.
- **Risk Management & 24h Hackathon Experience:**
  - *Git Security Slip-up:* Exhaustion led a member to push a `.env` environment file to Git, serving as a hard-learned lesson on secrets management.
  - *Time & Scope Management:* Expended 3 hours merely revising UI text, emphasizing the critical need for upfront role delegation and maintaining manageable project scope.

#### 6. Anti-Money Laundering (AML) Solution (Six Pillars - Outstanding FinTech Team)

**Six Pillars** solved operational bottlenecks in Banking and Financial Services (BFSI):

- **Banking AML Bottlenecks:** Traditional rule-based engines generate **90%–95% false positive alerts**. Analysts spend around $20–$25 and nearly 3 hours reviewing each case manually, leading to severe backlogs.
- **Three-Tier Intelligent Workflow:**
  - *Layer 1 (Fast Detection):* Kinesis Data Streams ingest real-time transactions, Lambda extracts features, and XGBoost on Bedrock rapidly filters transactions (forwarding only 5%–10% suspicious cases upward).
  - *Layer 2 (Agentic Investigation):* Deployed three specialized sub-agents: **KYC Profile Check**, **Money Flow Check** (detecting structuring/smurfing patterns), and **Sanction Check**. Legal and typology rules were retrieved from **Vector OpenSearch**.
  - *Layer 3 (Decision & Human-in-the-loop):* Utilized a two-stage LLM evaluation (first LLM proposes Dismiss/Hold/Escalate; second LLM acts as an LLM-as-a-Judge reviewer). Complex or sanctioned cases were routed directly to the analyst dashboard for final human sign-off.

---

### Key Takeaways

#### Design & Business Mindset
- **Focus on Real Problems:** AI architecture matters little unless it solves real user pain points. Always start from the problem before choosing tech.
- **Hackathon Scope Management:** Keep the scope manageable, deliver a smooth MVP, and present the core solution clearly during pitching.
- **Human-in-the-loop Governance:** In sensitive domains like Banking or F&B, AI handles data processing and recommendations, while humans retain final approval authority.

#### Technical Architecture & FinOps
- **Narrowing Context via Multi-Agent Systems:** Splitting systems into specialized sub-agents (Crawler, KYC, Money Flow...) narrows context windows, allowing small models for simple tasks to cut costs.
- **FinOps Awareness:** Pre-filter data with plain code before LLM calls, and use lightweight models (like transitioning from SageMaker to YOLO Small).
- **AWS Service Integration:** Mastered linking Kinesis, Step Functions, Lambda, Bedrock Agent Core Memory, DynamoDB, and Vector OpenSearch into a cohesive system diagram.

---

### Application to Work & Study

- **University Projects:** Apply multi-agent patterns and context window reduction strategies to software engineering coursework.
- **Cloud Cost Optimization:** Prioritize Serverless architectures and right-sized model configs when using AWS to prevent unnecessary spending.
- **Teamwork & Pitching:** Learn structured architecture mapping and concise slide design techniques from winning teams.

---

### Online Participation Experience

Watching on YouTube wasn't the same as being there in person, but hearing teams talk about 3 AM debug sessions or accidentally burning $48 on SageMaker made the event really engaging. It gave me a lot of clarity on delegating tasks to agents to save token costs and choosing models that fit the budget.

Even through video recordings and transcripts, I gained plenty of practical lessons on how teams package products and solve real problems.

---

### Lessons Learned & Personal Contributions

- **Lessons Learned:** Keep an active learning mindset, understand business requirements before diving into code, and stay flexible with technical choices.
- **Personal Contribution:** Reviewed full video subtitle transcripts and compiled this structured report to share multi-agent architecture patterns and FinOps experiences with the AWS FCJ community.

---

### Event Gallery

Below are selected photos of presentation slides and team project demos from the **AWS Agentic AI Buildweek Showcase & Sharing** event:

![Official event banner for FCAJ - Agentic AI Build Week 2026 at Bitexco Financial Tower](/images/4-EventParticipated/4.4-Event4/1.png)

![Keynote speaker Mr. Joseph Marazota (Head of Technology, AWS ASEAN) delivering the opening address alongside Mr. Nguyen Gia Hung (Head of SA, AWS Vietnam) on stage](/images/4-EventParticipated/4.4-Event4/2.jpg)

![Opening slide for the AI-Powered Conversation Ordering project presentation by 1st Prize winner One Team](/images/4-EventParticipated/4.4-Event4/3.png)

![AWS Cloud infrastructure architecture diagram incorporating VPC, ECS Fargate, Amazon Bedrock, ALB, Cognito, and PostgreSQL](/images/4-EventParticipated/4.4-Event4/4.png)

![Team Plan V presenting the Solution Architect Professional Native App project at the Hackathon](/images/4-EventParticipated/4.4-Event4/5.png)

![Presentation slide outlining the four stages of the hackathon journey "The Journey Ahead: Four stages of our hackathon"](/images/4-EventParticipated/4.4-Event4/6.png)

![Real-time video streaming architecture diagram utilizing Kinesis Video Streams, ECS Stream Processor, SageMaker Endpoint, and Bedrock AgentCore Runtime](/images/4-EventParticipated/4.4-Event4/7.png)

![Team Six Pillars presenting the Adaptive AML Workflow Engine solution automating anti-money laundering investigation workflows for banking](/images/4-EventParticipated/4.4-Event4/8.png)

![Investigative data enrichment report interface and legally compliant workflow for the Adaptive AML Workflow Engine project](/images/4-EventParticipated/4.4-Event4/9.png)

![Slide introducing Team Six Pillars members competing at the Agentic AI Build Week Hackathon](/images/4-EventParticipated/4.4-Event4/10.png)

> *The event provided practical architectural lessons that helped reinforce my system design thinking and cloud cost management on AWS.*
