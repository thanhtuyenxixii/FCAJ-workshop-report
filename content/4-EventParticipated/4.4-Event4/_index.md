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
| **Attendance Mode** | Online (Video recording analysis & subtitle review) |
| **Role** | Online Attendee |

### Event Objectives

The **AWS Agentic AI Buildweek Showcase & Sharing** was a comprehensive wrap-up and project showcase event for the *Agentic AI Buildweek 2026 Hackathon*, jointly organized by **AWS**, **JI Fund**, and the **FCAJ community**. This event served as a real-world playground for cloud engineers, developers, and tech students to transform bold AI Agent ideas into functional products capable of solving practical enterprise challenges.

The core objective of this session was to dissect multi-agent infrastructure architectures and practical product development workflows for AI agents across F&B, market intelligence, and banking/finance domains. In addition, the event provided authentic career guidance from AWS ASEAN leadership, hard-earned FinOps lessons on cloud cost optimization, strategies for handling low-resource languages like Vietnamese, and effective scope management techniques for hackathons.

### Speakers & Participating Teams

The event gathered distinguished speakers from AWS alongside the top winning teams from the hackathon:

| No. | Speaker / Team | Role / Award | Topic |
| :-: | :--- | :--- | :--- |
| 1 | **Joseph Marazota** | Head of Technology @ *AWS ASEAN* | *Opening Keynote: A 20-year tech journey, shifting to the AI Agent era, breaking mental models, and the Human-in-the-loop principle* |
| 2 | **Nguyen Gia Hung** | Head of Solution Architect @ *AWS Vietnam* | *Special Guest: AWS Vietnam leadership welcome & presenting certificates to winning teams* |
| 3 | **One Team** | 1st Prize Winner @ *AWS Track* | *AI-Powered Conversational Ordering: Multi-channel KFC ordering agent on Zalo/WhatsApp utilizing AWS Agent Core Memory, TinyFish scraper, and anti-hallucination guardrails* |
| 4 | **Final Scale (Signal C)** | 2nd Prize Winner @ *AWS Track* | *Multi-Agent Market Intelligence: Competitor & market signal analysis based on Value Creation Canvas integrated with LangFuse & Bedrock Guardrails* |
| 5 | **Six Pillars** | Outstanding Team @ *FinTech Track* | *Adaptive Workflow Engine for Anti-Money Laundering (AML): Automated AML investigation assistant for banking, reducing false positives by 90–95%* |

---

### Key Highlights

#### 1. Career Guidance & Mindset Shift from AWS ASEAN Leadership

The opening presentation offered inspiring perspectives through keynotes by **Mr. Joseph Marazota** (Head of Technology, AWS ASEAN) and **Mr. Nguyen Gia Hung** (Head of Solution Architect, AWS Vietnam).

- **Evolution of Software Release Cycles:** Reflecting on a 20-year career in technology, Mr. Joseph highlighted how banking software releases evolved from once a quarter to once every two weeks, and now down to automated continuous releases per minute powered by AI Agents.
- **Breaking Old Mental Models:** Young tech talent should not feel constrained by legacy practices or fear a lack of experience. Adopting new mental models and fresh perspectives is essential to driving innovation.
- **The "Human-in-the-loop" Philosophy:** Although Amazon operates over 1 million robots in fulfillment centers, "a robot without human programming and direction is merely a piece of useless hardware." Humans remain the essential decision-makers (*Human-in-the-loop*) throughout every innovation lifecycle.

#### 2. Multi-Channel KFC AI Ordering Chatbot – Winning Solution by One Team

**One Team** captured 1st Prize by addressing a critical friction point in the F&B industry.

- **Real-World Problem Analysis:** Learning from past failures like McDonald's AI drive-thru trial (where AI hallucination led to ordering 100 chicken nuggets accidentally), the team identified that requiring customers to download new apps, create accounts, and switch chat apps creates massive ordering friction.
- **Multi-Channel Zalo/WhatsApp Solution:** Bringing the ordering experience directly inside everyday messaging apps (focusing heavily on Zalo in Vietnam).
- **Breakthrough Technical Architecture:**
  - *Dynamic Menu Scraping:* Lacking official KFC APIs, the team leveraged **TinyFish** to scrape dynamic menu items directly from KFC's website and store them in AWS databases.
  - *Contextual Personalized Memory:* Implemented **AWS Agent Core Memory** to give each user a dedicated agent memory, enabling the bot to recall last week's orders for instant re-ordering.
  - *Low Latency & Cost Optimization:* Achieved fast 3–5 second response latency at a cost of roughly **$0.006 per order** (a 75% reduction in infrastructure costs compared to traditional serverless architectures).
  - *Last Verification Layer:* Introduced a final order confirmation step before payment execution to eliminate AI hallucination risks.

#### 3. Competitor Intelligence via Multi-Agent Systems – Final Scale (Signal C)

**Final Scale** (a student team from FPT) demonstrated an exemplary blend of business domain understanding and modern multi-agent architecture.

- **"Business-First" Mindset:** No matter how complex a technology is, it cannot overcome domain business boundaries. Over 70% of pitching success comes from how effectively the solution addresses real-world business pain points.
- **Value Creation & Delivery Canvas:** Adapted the traditional Business Model Canvas (which focuses heavily on revenue streams unsuitable for hackathon demos) into a focused Value Creation and Delivery framework.
- **Multi-Agent Architecture & Security:**
  - *Crawler Subagent:* Combined Apify (for static web data) and TinyFish (for deep dynamic web crawling).
  - *Token Optimization & Injection Defense:* Cleaned raw data using plain code prior to LLM processing to reduce token costs and prevent prompt injection attacks from external websites.
  - *Quality Evaluation via LangFuse:* Evaluated output quality using **LangFuse**. Low-scoring data triggered up to two retries to save costs; persistent low scores were stored in DynamoDB tagged for human review.
  - *Multi-Layer Security:* Enforced protection using **Bedrock Guardrails**, AWS Cognito, WAF, and Amplify.

#### 4. FinOps Lessons from the YOLO Demo Discussion

An insightful segment during the Q&A focused on model selection trade-offs during live demonstrations.

- The team shared their early mistake of hosting a large object tracking AI model on **Amazon SageMaker**, which accumulated a **$48 bill** in just 3 hours of testing.
- The team rapidly downgraded to **YOLOv26 Small**. The compact model size provided smooth execution, maintained high confidence scores (90%–97% for human detection), and minimized infrastructure costs—a valuable FinOps lesson for cloud engineering students.

#### 5. Adaptive Workflow Engine for Anti-Money Laundering (AML) – Six Pillars

**Six Pillars** presented an impressive enterprise-grade solution tailored for the Banking, Financial Services, and Insurance (BFSI) sector.

- **Banking AML Pain Points:** Traditional rule-based monitoring generates **90%–95% false positive alerts**. Manual review by financial analysts costs $20–$25 per case and takes nearly 3 hours, causing severe operational backlogs and team burnout.
- **Three-Tier Intelligent Workflow:**
  - *Layer 1 - Fast Detection:* Kinesis Data Streams ingest real-time transactions, Lambda performs feature engineering, and an XGBoost model on Bedrock rapidly scores risk (routing only 5%–10% suspicious cases upward).
  - *Layer 2 - Agentic Investigation:* Deployed three specialized sub-agents: **KYC Profile Check**, **Money Flow Check** (detecting structuring/smurfing patterns), and **Sanction Check** (cross-referencing sanction lists). Knowledge was retrieved from legal and typology vector stores in **Vector OpenSearch**.
  - *Layer 3 - Decision & Human-in-the-loop:* Utilized a two-stage LLM evaluation (first LLM proposes Dismiss/Hold/Escalate; second LLM acts as an LLM-as-a-Judge reviewer). Critical or sanctioned cases were escalated immediately to a human analyst dashboard for final sign-off.

---

### Key Takeaways

#### Design & Business Mindset
- **"Product-First & Business-First" Philosophy:** A well-architected AI system fails if it does not solve real business pain points. Engineering must always start from real-world problems before choosing technologies.
- **Hackathon Scope Control:** Avoid expanding project scope during limited hackathon timelines. Winning strategies focus on maintaining a tight, functional Minimum Viable Product (MVP) coupled with clear pitching delivery.
- **"Human-in-the-loop" Governance:** In sensitive domains like Banking or F&B, AI acts as an accelerator, but final approval remains with human operators for absolute system safety.

#### Technical Architecture & FinOps
- **Context Isolation via Multi-Agent Systems:** Learned to break down monolithic tasks into specialized sub-agents (Crawler, Profile, Money Flow, Sanction) to narrow Context Windows, assigning lightweight models to simple tasks and larger LLMs to complex reasoning.
- **Practical Cloud FinOps:** Continuously monitor cloud spending by filtering data prior to LLM calls and choosing right-sized models.
- **AWS Technology Integration:** Mastered combining AWS Kinesis, Step Functions, Lambda, Bedrock Agent Core Memory, DynamoDB, and Vector OpenSearch into a cohesive enterprise architecture.

---

### Application to Work & Study

- **Applying Multi-Agent Patterns:** Incorporate multi-agent modular design and context isolation concepts into university software engineering projects.
- **Practicing Cloud Cost Optimization:** Calculate estimated resource costs before deployment on AWS, prioritizing serverless options and lightweight models over costly dedicated instances.
- **Enhancing Teamwork & Pitching Skills:** Adopt structured architecture diagrams, clear slide presentation styles, and a collaborative "Roll together, learn together" team spirit.

---

### Online Participation Experience

Even though I attended the showcase via online stream recordings and subtitle transcripts, I felt the vibrant energy and creative passion of the participating student teams and cloud engineers.

Authentic stories about midnight coding sessions, spirited debates on architecture trade-offs, and humorous lessons on accidental cloud bills brought technology to life. The event provided immense inspiration and confidence for my senior year journey in cloud and AI engineering.

---

### Lessons Learned & Personal Contributions

- **Lessons Learned:** Technology mastery has no shortcuts. Embracing a *Lifelong Learner* mindset, understanding real-world business needs, and staying agile are essential traits for a future Cloud & AI Architect.
- **Personal Contribution:** Conducted an in-depth review of event video transcripts and documentation, synthesizing this comprehensive study report to share valuable multi-agent architectural insights and hackathon lessons with the AWS FCJ community.

---

### Event Gallery

Below are the actual photos captured during the presentations, architecture slides, and team project showcases at the **AWS Agentic AI Buildweek Showcase & Sharing** event:

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

> *This event provided invaluable architectural insights and inspiration, reinforcing my system design mindset and shaping my career roadmap in AWS Cloud and Agentic AI.*
