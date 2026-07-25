---
title: "Event 1"
date: 2026-05-23
weight: 1
chapter: false
pre: " <b> 4.1. </b> "
---

## Report: AWS Vietnam Community Day 2026

| Event Information | Details |
| :--- | :--- |
| **Event Name** | AWS Vietnam Community Day 2026 (Saturday Meetup) |
| **Date & Time** | May 23, 2026 (09:00 - 12:00) |
| **Location** | 26th Floor, Bitexco Financial Tower, Ho Chi Minh City |
| **Role** | Attendee |

### Event Objectives
The AWS Vietnam Community Day 2026 at Bitexco Financial Tower was an in-person meetup designed to connect learners and practitioners across the AWS cloud ecosystem in Vietnam.

The session focused on real-world large-scale system operations from companies like GoTymeX, VPBank, and VIB. As a senior tech student, attending helped me gain fresh insights into infrastructure optimization, Generative AI / Multi-Agent adoption, and practical cloud engineering workflows.

### Speakers

| No. | Speaker | Role | Topic |
| :-: | ------- | ---- | ----- |
| 1 | **Tinh Truong** | Platform Engineer @GoTymeX | *Context Is Everything – Making AI Actually Work for You* |
| 2 | **Pham Ng Hai Anh** | AWS Community Builder @G-AsiaPacificVietnam | *Friendly AI Assistant w/ Amazon Quick* |
| 3 | **Nguyen Tuan Thinh** | AWS Champion Instructor, 12x AWS Certified | *From Edge To Origin: CloudFront as Your Foundation* |
| 4 | **Team VIB** | AWS Track Winner @LotusHacks2026 | *36 hours with LotusHacks – Building UTMorpho* |
| 5 | **Duc Dao** | Solution Architect @CloudKinetics | *Non-Determinism of "Deterministic" LLM Settings* |
| 6 | **Vy Lam** | Sr. Business Systems Analyst @VPBank | *Enterprise-Grade Multi-Agent System* |

---

### Key Highlights

#### 1. Pain Points from Monolith Systems & Basic AI

Through the speaker presentations, many enterprises still face issues with legacy Monolith setups or serving content directly from a single origin server without a CDN:

- **High Latency:** Users in Vietnam querying origin servers in the US suffer >200ms round trips, wasting international bandwidth and slowing page loads.
- **Security Vulnerabilities:** Direct public access to the origin server leaves systems exposed to DDoS attacks and botnets due to a lack of edge protection.
- **Limitations of Basic AI:** Sending raw prompts without context or memory management leads to hallucinations and unreliable responses for complex workflows.
- **Misconception about `Temperature = 0`:** Setting temperature to 0 does not guarantee 100% deterministic outputs due to asynchronous GPU parallel optimization (Non-Determinism).

#### 2. Microservices & Multi-Agent Applications in Banking

Migrating from Monoliths to Microservices decouples large monoliths into independent services communicating via APIs or events.

A key takeaway from Ms. Vy Lam (VPBank) was applying microservice principles to AI via **Multi-Agent** systems:
- Rather than relying on a single LLM to handle everything, tasks are split across specialized agents: data collection, credit risk analysis, and compliance checking.
- Modular agents improve fault isolation and allow independent component updates.

#### 3. Domain Boundary Mapping with DDD

Domain-Driven Design (DDD) establishes clear boundaries for microservices or individual AI agents. Engineers collaborate with business stakeholders to define a Ubiquitous Language and map out Bounded Contexts.

- **Lesson from Team VIB (LotusHacks 2026):** During the 36-hour sprint building UTMorpho, applying DDD to separate business boundaries kept the team focused on delivering a functional MVP on time.
- **Enterprise Use Case at VPBank:** Setting up a "Virtual Credit Committee" using Multi-Agents required distinct agent responsibility boundaries to adhere to strict banking compliance.

#### 4. Latency Optimization with Event-Driven Architecture & Edge Computing

- **Event-Driven:** Services and agents publish and consume events asynchronously, decoupling system dependencies.
- **Edge Processing:** Combining Amazon CloudFront with CloudFront Functions and Lambda@Edge inspects requests and responses directly at nearby Edge Locations. Performing URL rewrites or header modifications at the Edge achieves sub-1ms latency without routing back to distant origin servers.

#### 5. Compute Evolution: From Bare-Metal to Serverless

Looking at compute evolution over time: Bare-metal physical servers -> EC2 Virtual Machines -> Containers (ECS, Fargate) -> Serverless (AWS Lambda).

Serverless eliminates OS management and server patching overhead. Systems scale automatically based on traffic and bill strictly for execution time, enabling developers to focus purely on business logic.

#### 6. Accelerating SDLC with AI Assistants (Amazon Q & Quick Suite)

Amazon Q Developer and the Amazon Quick suite (Quick Chat, Quick Flow, Quick Spaces, Quick Sight) streamline software development lifecycle workflows:

- Speeds up code generation, refactoring, and unit test creation.
- Quick Sight enables non-technical business users to run natural language queries against raw data to build visual analytics dashboards without writing SQL.

---

### Key Takeaways

#### Design & Architecture Mindset
- CloudFront is more than a static asset cache; it acts as an edge security shield (Origin Cloaking) protecting backend origin servers.
- AI applications require structured context and long-term memory (via RAG) to produce domain-accurate responses.
- System design must remain business-first; pushing overly complex tech into simple problems creates unnecessary overhead.

#### CloudFront & LLM Engineering
- Edge Tooling Trade-offs:
  - *CloudFront Functions:* Ultra-fast (<1ms) lightweight JS execution at the edge, ideal for header rewrites and redirects.
  - *Lambda@Edge:* Supports full Node.js/Python runtimes for complex business logic.
- Understanding LLM non-determinism (floating-point operations on parallel GPUs) and employing Guardrails to enforce structured outputs.

---

### Application to Work & Study

- Position CloudFront in front of backend origins in upcoming projects (e.g., IoT Weather Platform) to accelerate data transfers and secure origins using OAC / AWS Shield.
- Deploy static S3 front-ends paired with CloudFront and Origin Access Control (OAC) to block public S3 bucket access.
- Integrate Amazon Q Developer into VS Code for code generation and unit testing.
- Build a personal RAG-based AWS study chatbot to index and search study notes.

---

### Event Experience & Discussions

- **Speaker Insights:** Impressed by Mr. Nguyen Tuan Thinh's CloudFront session. As an AWS Champion Instructor, his practical analogies made CDN concepts and local Edge Locations easy to grasp.
- **Live Demos:** Watching VPBank's Multi-Agent credit evaluation demo and hearing Team VIB recount their LotusHacks last-minute bug fixes offered valuable real-world engineering perspectives.
- **Networking:** Spoke with senior systems engineers and AWS Community Builders during breaks to gather practical tips on career growth and AWS certification prep.

---

### Lessons Learned & Personal Engagement

- **Lessons Learned:** Clear technical communication relies on mapping abstract concepts to relatable real-world analogies. Edge optimization and contextual AI integration are essential foundations for modern cloud architectures.
- **Personal Engagement:** Raised questions during the CloudFront talk to clarify trade-offs between CloudFront Functions and Lambda@Edge, and connected with peers in the AWS FCJ community.

---

### Event Gallery

Below are photos captured during presentation slides and key moments at AWS Vietnam Community Day 2026:

![Slide "What's Next" for UTMorpho project by Team VIB, showing QR codes to access their Devpost and GitHub repositories](/images/4-EventParticipated/4.1-Event1/IMG20260523105858.jpg)

![Slide explaining LLM configuration parameters: Top-P and Top-K with a QR code for interactive visualization](/images/4-EventParticipated/4.1-Event1/IMG20260523111358.jpg)

![Slide showing the research reality: performance of LLMs tested on various NLP tasks](/images/4-EventParticipated/4.1-Event1/IMG20260523111644.jpg)

![Slide explaining the technical root cause of LLM non-determinism, highlighting floating-point non-associativity in IEEE 754 on GPUs and parallel thread execution order](/images/4-EventParticipated/4.1-Event1/IMG20260523112629.jpg)

![Slide proposing mitigation strategies for LLM non-determinism, including multiple runs/voting, structured outputs, and self-hosted models](/images/4-EventParticipated/4.1-Event1/IMG20260523112744.jpg)

![Slide summarizing LLM configuration tips: greedy decoding loop risks, the temperature 0.1 sweet spot, and repeat penalty settings](/images/4-EventParticipated/4.1-Event1/IMG20260523113044.jpg)

![Slide "Tips" for LLM configuration, featuring red handwritten overlay notes emphasizing setting temperature to 0.1 to avoid repetition](/images/4-EventParticipated/4.1-Event1/IMG_20260523_113141.jpg)

![Slide showing the Key Takeaways: designing applications with variance in mind and focusing on thorough testing](/images/4-EventParticipated/4.1-Event1/IMG20260523113240.jpg)

![Slide displaying a QR code referencing the research paper "LLM Settings" by Rebecca J. Passonneau](/images/4-EventParticipated/4.1-Event1/IMG20260523113404.jpg)

![Slide showing the Agenda for enterprise-grade Multi-Agent architecture for credit assessment](/images/4-EventParticipated/4.1-Event1/IMG20260523114740.jpg)

![Slide explaining why multi-agent architecture works, highlighting specialized expertise, checks & balances, and fault tolerance](/images/4-EventParticipated/4.1-Event1/IMG20260523120206.jpg)

![Slide outlining the six pillars of enterprise-grade AI: Security, Data Governance, Network, Operations, Human Factors, and Compliance](/images/4-EventParticipated/4.1-Event1/IMG20260523120612.jpg)

![Slide presenting the proposed deployment approach, detailing steps from local app to AWS services](/images/4-EventParticipated/4.1-Event1/IMG20260523121833.jpg)

![Architecture diagram showing the basic deployment flow from the local developer environment to AWS cloud infrastructure](/images/4-EventParticipated/4.1-Event1/IMG20260523121900.jpg)

![Slide listing the workshop exercises: configuring authentication, implementing Guardrails, MCP, and Terraform](/images/4-EventParticipated/4.1-Event1/IMG20260523122352.jpg)

![Slide listing workshop exercises, overlayed with a red handwritten note highlighting that building a system is not just about making it run, but making it run securely](/images/4-EventParticipated/4.1-Event1/IMG_20260523_123205.jpg)

![Group photo of all participants and speakers at the end of the event](/images/4-EventParticipated/4.1-Event1/event1.jpg)

> *The meetup provided practical architectural perspectives, helping reinforce my cloud learning path across AWS and Generative AI.*
