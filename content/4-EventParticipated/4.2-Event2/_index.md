---
title: "Event 2"
date: 2026-06-06
weight: 2
chapter: false
pre: " <b> 4.2. </b> "
---

## Report: AWS First Cloud Journey AI

| Event Information | Details |
| :--- | :--- |
| **Event Name** | AWS First Cloud Journey AI |
| **Date & Time** | June 6, 2026 (09:00 - 12:00) |
| **Location** | 26th Floor, Bitexco Financial Tower, Ho Chi Minh City |
| **Role** | Attendee |

### Event Objectives
The **AWS First Cloud Journey AI** event took place at the AWS Vietnam office (26th floor, Bitexco Financial Tower). This session focused on AI, cloud-native application patterns, teamwork practices, and career paths for students and junior developers.

The agenda covered real-time multiplayer game design, container optimization with Docker, GraphRAG search patterns, web security via Machine Learning, and career progression from Helpdesk to Sysadmin.

### Speakers

| No. | Speaker | Role | Topic |
| :-: | ------- | ---- | ----- |
| 1 | **Nguyen Quoc Bao** | Cloud Engineer / Game Developer | *Multiplayer in the Cloud: Connecting Godot Clients with AWS WebSockets* |
| 2 | **Huynh Bao** | Junior Cloud Native Developer @Endava Vietnam | *Docker — A containerization technology* |
| 3 | **Viet Phat** | AI Majoring @Swinburne University of Technology | *GraphRAG: Build GraphRAG applications using Amazon Bedrock and Amazon Neptune* |
| 4 | **Le Hoang Gia Dai** | Senior Student @HUTECH University | *WAF + ML for Cyber Attack Detection: Machine Learning-based Network Intrusion Detection System (NIDS) on AWS* |
| 5 | **Tran Trung Vinh** | System Administrator @Central Retail Group | *From IT Helpdesk to Senior Sysadmin* |
| 6 | **Truong Huy Phuoc** | Presenter / Teamwork Coach | *The Art of Effective Teamwork* |

---

### Key Highlights

#### 1. AWS AI Leadership Panel Discussion

Key takeaways from the opening panel on learning and applying AI effectively:

- **Design Mindset Beats Raw Coding:** With AI assistants generating code snippets, an engineer's core value lies in system architecture design, problem decomposition, and cloud service integration. Manual coding is being automated, but business analysis remains key.
- **Bridging Enterprise Gaps:** Companies want AI adoption quickly, but engineering teams often lack practical cloud execution skills. Levering AWS Managed Services reduces delivery cycles and cuts operational overhead.
- **Opportunities for Students:** Cloud and AI have lowered technical barriers. With sound design thinking and solid ideas, students can build global-scale applications independently.

#### 2. Multiplayer Gaming with Godot & AWS WebSockets

This talk demoed a real-time rock-paper-scissors multiplayer game with key engineering notes:

- **Network Protocol Trade-offs:**
  - *UDP/ENet:* Ultra-low latency (ideal for FPS/racing), but requires custom application-level reliability handling.
  - *HTTP Polling:* Easy to set up, but causes high latency and bandwidth overhead due to continuous polling.
  - *WebSocket:* Optimal for turn-based games and matchmaking lobbies, providing full-duplex two-way communication.
- **AWS Architecture:** Godot client uses `WebSocketPeer` to connect to API Gateway WebSocket. The API Gateway routes requests using `$request.body.action` to AWS Lambda (Node.js 20), which updates game states in DynamoDB.
- **Real-World Engineering Challenges:**
  - *Stale Connections:* Sudden player drop-offs leave orphaned `connectionId` records in DynamoDB, throwing `GoneException` errors when pushing updates.
  - *DynamoDB Scan Costs:* Using `ScanCommand` for matchmaking triggers full table scans, growing costly and sluggish as user counts scale.
  - *Stateless Lambda:* Stateless Lambda requires fetching and persisting game session state on every execution loop.
- **Scaling Path:** Transition to AWS GameLift when dedicated servers are required for complex physics calculations.

#### 3. Containerization Notes with Docker

Speaker Huynh Bao covered containerizing applications with Docker to solve the classic "works on my machine" deployment issue:

- **VM Drawbacks:** Traditional VMs run full guest OS instances, consuming heavy CPU, RAM, and disk resources.
- **Docker Benefits:** Packages an app and dependencies into a lightweight container sharing the host OS kernel.
- **Image Layers:** Each line in a `Dockerfile` generates an image layer. Docker reuses cached layers to speed up rebuild times.
- **Applications:** Forms the foundation for microservices and automated CI/CD deployment pipelines.

#### 4. GraphRAG Applications with Amazon Bedrock & Neptune

Speaker Viet Phat introduced graph-structured data retrieval to optimize LLM query contexts:

- **Basic RAG Bottlenecks:** Traditional vector search struggles with multi-step queries requiring multi-hop reasoning.
- **GraphRAG Approach:** Constructs Knowledge Graphs with entity nodes and relationship edges. When queried, LLMs traverse the graph across multiple documents to extract connected context.
- **AWS Deployment Routes:**
  - *Fully Managed:* Uses Amazon Bedrock Knowledge Bases for automated chunking, entity extraction, and graph storage in Amazon Neptune Analytics.
  - *Custom Route:* Builds custom pipelines using LlamaIndex and Amazon Neptune, querying graph paths via Cypher Query.

#### 5. Web Security: Combining WAF + Machine Learning (NIDS)

Particularly impressed by the WAF + ML talk from Gia Dai (a fellow HUTECH senior student). His approach to handling imbalanced datasets from CSE-CIC-IDS2018 and streaming logs via Kinesis Firehose to S3 was practical and smooth:

- **Traditional WAF Limits:** Static rule-based WAFs struggle to catch novel zero-day attacks or abnormal traffic patterns without known signatures.
- **WAF + Machine Learning Solution:**
  - Trains a LightGBM model on CSE-CIC-IDS2018 datasets on AWS for Network Intrusion Detection (NIDS).
  - Preprocessing pipeline: Merges large CSV files, cleans corrupted data (NaN, negatives, infinities), and balances minority attack classes before training.
- **AWS Architecture Flow:** VPC hosts EC2 running NIDS behind an ALB. Traffic logs stream via Kinesis Data Firehose to S3 -> Lambda analyzes log predictions -> SNS alerts push to Security Hub, GuardDuty, and CloudWatch.

#### 6. Career Progression: From Helpdesk to Senior Sysadmin

Speaker Tran Trung Vinh (Central Retail) shared practical lessons on self-learning and career advancement:

- **Skills from Helpdesk:** Teaches troubleshooting under pressure, customer communication, and root-cause analysis.
- **Turning Point:** Deep-dived into Linux and Networking fundamentals, building virtualized home labs at home.
- **Sysadmin Principles:**
  - Strict rule: *"Never test directly in Production"*.
  - Shifted to Cloud (AWS), Infrastructure as Code (Terraform), and DevOps automation pipelines.

#### 7. Teamwork Practices & Digital Tools

Speaker Truong Huy Phuoc outlined 4 golden rules for effective teamwork:
1. *Clear Goals:* Aligning team members on a shared destination.
2. *Right Place:* Assigning tasks based on individual strengths.
3. *Open Communication:* Respectful feedback and active listening.
4. *Accountability:* Taking ownership of individual deliverables.

Collaboration Tools: Trello/ClickUP for task management; Slack/Discord/Google Workspace for communication and documentation.

---

### Key Takeaways

#### Technical Mindset
- Combining Knowledge Graph Databases (Amazon Neptune) with LLMs via GraphRAG provides richer context retrieval than plain vector search.
- Real-time application design requires picking the right network protocol (WebSocket vs UDP) and handling abrupt disconnection cleanup (stale connections).
- Modern web security benefits from combining ML with WAF for proactive intrusion detection.

#### Engineering Skills & Tools
- Understanding API Gateway WebSocket Route Keys and `connectionId` management in DynamoDB.
- Leveraging Dockerfile image layer caching to optimize build speeds.
- Self-learning lesson from Vinh: master Linux & Networking fundamentals before spending on expensive cloud tools.

---

### Application to Work & Study

- Write Dockerfiles to package university web projects, optimizing image layer caching and pushing images to Amazon ECR.
- Build a local Linux/Networking home lab to write shell scripts for task automation.
- Build a simple Godot multiplayer game connected to AWS API Gateway WebSocket and DynamoDB.
- Apply Trello and Slack tools to manage HUTECH group projects effectively.

---

### Event Experience & Discussions

- **Practical Perspectives:** Most impressed by Gia Dai's (HUTECH) WAF + ML presentation and Vinh's journey from Helpdesk to Sysadmin through self-built home labs.
- **Live Demos:** Bao's Godot rock-paper-scissors demo over WebSocket illustrated how `connectionId` records are managed in DynamoDB in real-time.
- **Networking:** Spoke with Huynh Bao during breaks about working at Endava Vietnam and his student initiative at ITea Lab.

---

### Lessons Learned & Personal Engagement

- **Lessons Learned:** The best solution is right-sized for the business problem rather than overly complex. A solid Linux and Networking foundation speeds up cloud learning immensely.
- **Personal Engagement:** Asked speaker Viet Phat about cost and latency trade-offs between Custom GraphRAG and Managed Bedrock Knowledge Bases; networked with fellow student attendees.

---

### Event Gallery

Below are photos captured during presentation slides and key moments at AWS First Cloud Journey AI:

![Slide showing three discussion questions for the AWS AI Director panel on AI trends, critical skills for young developers, and bridging enterprise capability gaps](/images/4-EventParticipated/4.2-Event2/IMG20260606090407.jpg)

![AWS AI Director presenting on how cloud computing and AI are lowering the barrier of entry for young technology builders](/images/4-EventParticipated/4.2-Event2/IMG20260606091540.jpg)

![Title slide of the presentation 'Multiplayer in the Cloud: Connecting Godot Clients with AWS WebSockets' by speaker Nguyen Quoc Bao](/images/4-EventParticipated/4.2-Event2/IMG20260606092843.jpg)

![Agenda slide (Table of Contents) for the multiplayer game WebSocket connection session](/images/4-EventParticipated/4.2-Event2/IMG20260606092927.jpg)

![Slide detailing the DynamoDB Database Schema design for storing player and opponent matchmaking states](/images/4-EventParticipated/4.2-Event2/IMG20260606093539.jpg)

![Slide summarizing technical challenges including Stale Connections, DynamoDB Scan costs, and Lambda statelesness](/images/4-EventParticipated/4.2-Event2/IMG20260606095009.jpg)

![Title slide of the presentation 'Docker — A containerization technology' by speaker Huynh Bao](/images/4-EventParticipated/4.2-Event2/IMG20260606095859.jpg)

![Agenda slide outlining the main sections of the Docker sharing session](/images/4-EventParticipated/4.2-Event2/IMG20260606100049.jpg)

![Slide analyzing the benefits of virtualization and the need for containerization to optimize server resources](/images/4-EventParticipated/4.2-Event2/IMG20260606100430.jpg)

![Slide comparing Virtual Machines vs Containers in terms of system architecture, size, and resource footprint](/images/4-EventParticipated/4.2-Event2/IMG20260606101230.jpg)

![Kasane Teto themed slide wrapping up the Docker presentation by Huynh Bao](/images/4-EventParticipated/4.2-Event2/IMG20260606103701.jpg)

![Agenda slide introducing the structure of the GraphRAG session by speaker Viet Phat](/images/4-EventParticipated/4.2-Event2/IMG20260606103959.jpg)

![Title slide of the session 'WAF + ML for Cyber Attack Detection: Machine Learning-based Network Intrusion Detection System (NIDS) on AWS' by speaker Le Hoang Gia Dai](/images/4-EventParticipated/4.2-Event2/IMG20260606105006.jpg)

![Slide explaining web application protection capabilities of AWS WAF (Web Application Firewall)](/images/4-EventParticipated/4.2-Event2/IMG20260606105132.jpg)

![System diagram showing how NIDS works, connecting web traffic, machine learning models, databases, and alerting](/images/4-EventParticipated/4.2-Event2/IMG20260606105920.jpg)

![Cloud architecture diagram detailing integration of AWS WAF, Lambda, S3, Kinesis Firehose, CloudWatch, and AWS security services](/images/4-EventParticipated/4.2-Event2/IMG20260606110049.jpg)

![Slide summarizing experimental results, future improvements with Bedrock, and lessons learned by Le Hoang Gia Dai](/images/4-EventParticipated/4.2-Event2/IMG20260606110335.jpg)

![Title slide of the session 'From IT Helpdesk to Senior Sysadmin' by speaker Tran Trung Vinh at Central Retail Group](/images/4-EventParticipated/4.2-Event2/IMG20260606111037.jpg)

![Agenda slide outlining the career progression path from Helpdesk to Sysadmin and Cloud/DevOps](/images/4-EventParticipated/4.2-Event2/IMG20260606111212.jpg)

![Slide highlighting skills learned from Helpdesk and the turning point of building hands-on labs for Linux & Networking](/images/4-EventParticipated/4.2-Event2/IMG20260606111433.jpg)

![Slide detailing Sysadmin role realities and key lessons about never testing on production environments](/images/4-EventParticipated/4.2-Event2/IMG20260606111742.jpg)

![Slide illustrating the Modern DevOps Roadmap with eight key stages from Linux to Monitoring & Observability](/images/4-EventParticipated/4.2-Event2/IMG20260606112554.jpg)

![Contact details and connection QR code slide for speaker Tran Trung Vinh](/images/4-EventParticipated/4.2-Event2/IMG20260606113609.jpg)

![Photo of my hand-written notebook summarizing speaker details and sequence during the event](/images/4-EventParticipated/4.2-Event2/note.png)

> *The event provided valuable real-world insights, helping me better connect AWS technology concepts and boosting my learning motivation.*
