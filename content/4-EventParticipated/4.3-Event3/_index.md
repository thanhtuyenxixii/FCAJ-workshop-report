---
title: "Event 3"
date: 2026-06-27
weight: 3
chapter: false
pre: " <b> 4.3. </b> "
---

## Report: AWS FC Community Day – Solutions for Enterprise AI Agents & Security

| Event Information | Details |
| :--- | :--- |
| **Event Name** | AWS FC Community Day 2026 |
| **Date & Time** | June 27, 2026 (09:00 - 12:00) |
| **Location** | 26th and 36th Floor, Bitexco Financial Tower, Ho Chi Minh City |
| **Attendance Mode** | Online (YouTube Livestream) |
| **Role** | Attendee |

### Event Objectives

The **AWS FC Community Day** was a monthly technology seminar series connecting developers and students with real-world enterprise engineering challenges.

This session focused on enterprise AI Agent deployment patterns: from Multi-Agent cloud infrastructure monitoring and FinOps cost optimization, real-time Vietnamese Voice AI for banking contact centers, DevOps incident response automation, to HR automation via Amazon Q and private security connectivity over VPC Endpoints with MCP servers.

### Speakers

| No. | Speaker | Role | Topic |
| :-: | :--- | :--- | :--- |
| 1 | **Steve Tran** | Founder @ *Cloud Thinker* (Ex-AWS Solution Architect) | *Agentic Platform for Cloud Infrastructure: Cloud career progression, Single-Agent vs. Multi-Agent architectures, and FinOps cost optimization* |
| 2 | **Hieu Nghi, Kiet & Trung Do** | Cloud Engineers @ *Renova Cloud* & CEO @ *R AI* | *Voice AI Agents: Building intelligent voice agent systems, real-time Vietnamese speech processing, and banking tool calling scenarios* |
| 3 | **Nguyen Nguyen & Bao** | Cloud Engineers @ *Cloud Kinetics* | *DevOps AI Agent: Automating categorization, root cause investigation, and reducing MTTR/MTTD of infrastructure via Agent Space* |
| 4 | **Truong (Wren) & Minh Anh** | AI Solutions Team @ *Noventics* | *HR Intelligent Automation via Amazon Q: Achieving 99% accuracy in CV parsing, rating candidates based on competency framework, and building no-code pipelines* |
| 5 | **Toan Nguyen & Hieu Nghi** | AWS Security Builder @ *AWS Community* | *Private Security for AI Agents: Establishing secure private network connectivity between Amazon Q and MCP servers via VPC Endpoints* |

---

### Key Highlights

#### 1. Multi-Agent Systems & FinOps (Steve Tran)

Speaker Steve Tran shared his career journey and practical approaches to managing cloud infrastructure complexity as systems scale:

- **Career Path Story:** Shared dropping out of college at 19 to work at a Contact Center (2018-2019), struggling with physical server hardware failures. Realizing foundational gaps and failing Azure exams 3-4 times, he redirected his focus to mastering AWS documentation, eventually becoming an AWS Solution Architect before founding Cloud Thinker.
- **Addressing Tech Debt:** In large banking/finance systems, legacy technical debt makes manual incident investigation take hours, while AI can analyze logs in minutes.
- **Single-Agent vs Multi-Agent:** While a well-designed Single Super Agent can handle ~95% of tasks, Multi-Agent architectures narrow context windows, allowing smaller, low-cost models for simple tasks and supporting strict RBAC.
- **Approval Layers:** To prevent automated scripts from accidentally mutating production databases, Cloud Thinker's system enforces multi-layer approvals (Layer Approval) before applying production changes, alongside automated FinOps optimization.

#### 2. Vietnamese Voice AI for Contact Centers (Team R AI)

Team R AI and Renova Cloud demonstrated building intelligent Vietnamese voice assistants for banking environments (VPBank, VIB):

- **Low-Resource Language Challenge:** Existing Speech-to-Speech models primarily target English, whereas Vietnamese lacks large training corpora.
- **3-Step Pipeline:** Speech-to-Text (STT) -> LLM -> Text-to-Speech (TTS). STT streams audio into text for the LLM, which generates text responses based on banking prompts, passing text directly to TTS for audio output.
- **Handling Vietnamese Language Nuances:**
  - *Gender Address:* Predicting gender from voice acoustics to choose appropriate address pronouns (anh/chị).
  - *Interruption Algorithm:* Training auxiliary models to detect natural pauses (e.g., stopping while reading a phone number), preventing the AI from interrupting or talking over customers.
  - *Regional Accents:* Training with 10%-20% Central and Northern regional accents to maintain recognition accuracy.
- **Tool Calling Integration:** Enables direct execution (e.g., verifying ID and issuing card locks in real-time). If a caller becomes frustrated, the system seamlessly transfers the call to a human operator.

#### 3. Incident Response Automation with DevOps AI Agent (Cloud Kinetics)

Cloud Kinetics introduced automated root cause analysis (RCA) tools to relieve pressure on SRE/DevOps teams during critical outages:

- **Data Fragmentation Pain Point:** Manually searching logs across CloudWatch and CloudTrail causes context loss and extends MTTD/MTTR.
- **6 Core Pillars of DevOps AI Agent:**
  1. *Context Learning:* Uses Agent Space (tag-defined logical resource containers) to automatically map system topology.
  2. *Control:* Restricts agent permissions via tags or private connections.
  3. *Integration:* Leverages Model Context Protocol (MCP) to query databases directly for evidence.
  4. *Collaboration:* Interacts via Web UI, Slack, or ServiceNow.
  5. *Convenience:* Fast activation within AWS Console.
  6. *Cost-effective:* Billed by execution seconds (~$0.083/second).
- **4-Step Workflow:** Alert Trigger -> Hypothesis Generation -> Log Verification -> Mitigation Recommendation (leaving execution to human operators for Safety First).
- **Case Studies:** WGU reduced incident resolution times from 2 hours to 28 minutes (77% MTTR reduction). Zenchef reduced misconfiguration lookup times by 75% down to 20 minutes.

#### 4. HR Automation with Amazon Q (Team Noventics)

Team Noventics presented an AI solution streamlining non-technical HR workflows:

- **Manual HR Friction:** Slow manual CV screening risks missing top talent, subjective bias distorts scoring, and uploading internal HR data to public LLMs causes privacy risks.
- **Amazon Q Solution:** Connects to Google Workspace, SharePoint, OneDrive, Gmail, S3 with data protected in Vietnam Local Zones.
- **Automation Pipeline:**
  - *Skill Learning:* Ingests a `.md` document to generate an *HR Talent Review Assistant* skill.
  - *CV Screening & Scoring:* Scans candidate resumes (99% OCR accuracy), compares against JDs, and outputs HTML Talent Review reports with fit ratings (*Strong, Good, Low, Very Low*) and salary benchmarks.
  - *Workflow Execution:* Checks hiring manager calendar availability to book interviews and draft email responses automatically.

#### 5. Private Security for AI Agents via VPC Endpoints (Toan Nguyen & Hieu Nghi)

The final security deep-dive focused on protecting data when AI agents connect to third-party tools:

- **Public Endpoint Risks:** Exposing Amazon Q connections to external MCP servers (Zalo, WhatsApp, Jira) over the public internet risks Man-in-the-Middle and DDoS attacks.
- **Closed Network Architecture:** Implements Zero Trust by isolating all MCP servers within Private Subnets.
- **Security Flow:** Amazon Q -> VPC Connection (via Interface Endpoints / AWS PrivateLink) -> Cognito Authentication -> ALB (TLS encrypted via ACM) -> Route 53 Resolver for internal DNS -> MCP Server. Traffic stays 100% private within AWS infrastructure.

---

### Key Takeaways

#### Technical & Architecture Mindset
- Enterprise systems require defense-in-depth network isolation (VPC Endpoints) rather than just functioning code.
- Human-in-the-loop: For production changes (DevOps agent patches) or financial decisions (FinOps), AI recommends while humans approve.
- Multi-Agent decoupling narrows context windows and cuts costs by assigning smaller models to simpler tasks.
- Mastered the 3-step Voice AI streaming pipeline (STT -> LLM -> TTS) and MCP Server integrations.

#### Career Lessons
- Steve Tran's advice: Mastering Linux and Networking fundamentals is essential before moving into higher-level cloud and AI tooling.

---

### Application to Work & Study

- Use Docker to package course projects, applying least-privilege tagging similar to DevOps Agent Spaces.
- Try the DevOps AI Agent on AWS Console to observe automatic topology mapping and log analysis.
- Build a personal Amazon Q chat agent by loading `.md` course slides to quickly search exam topics.

---

### Event Experience & Discussions

- **Speaker Insights:** Liked Steve Tran's transparent sharing about early career failures and trade-offs when selecting customer architectures.
- **Live Demos:** Watching Kiet's Bedrock Agent Core voice demo and Trung Do's breakdown of Vietnamese text streaming handling interruptions provided solid real-world context.
- **Practical Application:** Noventics' Amazon Q CV parsing demo and Toan Nguyen's private network cost breakdown ($250-$350/month for ALB, Route 53 Resolver, EC2) gave realistic solution architecture perspectives.
- **Networking:** Followed up on Livestream by connecting with Toan Nguyen on LinkedIn for VPC Connection documentation.

---

### Lessons Learned & Personal Engagement

- **Lessons Learned:** Successful enterprise systems solve real business pain points (MTTR reduction, data security) cost-effectively. Solid Linux & Networking foundations remain key.
- **Personal Engagement:** Asked speaker Toan Nguyen via Livestream chat about private network operational costs in production; connected with fellow student attendees on LinkedIn.

---

### Event Gallery

Below are photos captured during the AWS FC Community Day 2026 event:

![Opening slide introducing the monthly AWS FC Community Day event series at Bitexco](/images/4-EventParticipated/4.3-Event3/IMG20260620130023.jpg)

> *The event provided practical insights into deploying AI Agents and securing cloud infrastructure on AWS.*
