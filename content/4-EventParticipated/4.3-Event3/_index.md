---
title: "Event 3"
date: 2026-06-27
weight: 3
chapter: false
pre: " <b> 4.3. </b> "
---

# Summary Report: “FCAJ Community Day – June 27, 2026”

### Event Objectives

- Get up to date on applying AI agents to cloud infrastructure operations (CloudOps, DevOps) in the enterprise.
- Learn about Voice AI and the specific challenges of building voice assistants for Vietnamese.
- Introduce Amazon Quick — AWS's agentic AI tool — and how it applies to HR recruitment.
- Share a solution for setting up a private, secure connection between an AI agent and an MCP server in an enterprise environment.

### Speakers

- **Steve Tran** – CTO/Founder, Cloud Thinker
- **Nghi Danh** – AI Engineer, Renova Cloud
- **Kiet Tran** – AI Engineer, AWS Student Builder Group
- **Trung Vu** – CEO, Revve AI
- **Bao Phan** – Cloud Engineer, Cloud Kinetics
- **Nguyen Nguyen** – Cloud Engineer, Cloud Kinetics
- **Truong Tran** – AI Solution Sales, Noventiq
- **Anh Dang** – Solution Sales, Noventiq
- **Toan Nguyen** – AWS Security Builder

### Key Highlights

#### Cloud Thinker – An agentic platform for cloud operations

- Cloud Thinker is an agentic platform that supports three main problems: fast incident handling, automated code review, AI-driven cost optimization (FinOps), and security testing (pen-testing).
- **Single-agent vs. multi-agent architecture:** A single agent can handle most tasks, while a multi-agent architecture is more cost-, context-, and access-control (role-based access control) efficient once the system gets complex.
- Move fast and stay close to the real-world problems of strategic "champion customers" (large enterprises).

#### Voice AI for Vietnamese

- Introduced two Voice AI architectures: direct speech-to-speech and a three-tier pipeline (speech-to-text → LLM → text-to-speech).
- Vietnamese is a low-resource language for training, so current speech-to-speech models don't yet perform well; a real banking solution instead uses the three-tier architecture to keep content and tool-calling under control.
- Vietnamese-specific challenges: detecting the speaker's gender for correct pronoun use, handling natural interruptions, recognizing regional accents, plus operational requirements like audit logs, versioning, and human handoff.

#### DevOps AI Agent

- Six pillars of a DevOps agent: Context Learning, Control, Integration (via MCP), Collaboration, Convenience, and Cost-effectiveness (billed by runtime).
- **A 4-step DevOps AI agent workflow:** Triage → Investigation (form and verify hypotheses) → Mitigation (propose a fix, don't auto-execute) → Improvement.

#### Amazon Quick applied to HR

- Manual CV screening easily misses strong candidates, lacks a standardized scoring rubric, carries security risk when public AI tools are used, and drags out time-to-hire.
- Amazon Quick is an agentic AI that automates CV screening, analyzes candidate data against the job description (JD), scores candidates, and streamlines the interview process.

#### A secure connection between Amazon Quick and an MCP server

- The security problem of connecting an AI agent to a third-party MCP server over the public internet (DDoS and man-in-the-middle risk).
- Solved by setting up the MCP server to connect Amazon Quick to third-party data sources privately, without going through the public internet, to strengthen enterprise security.

### Key Takeaways

#### Growth mindset

- AI amplifies human capability rather than fully replacing it, especially for roles that require real hands-on operational experience.
- Recognized the importance of getting real-world experience early (internships, projects) to keep up with a job market that's shifting because of AI.

#### Technical architecture

- Understood the difference between, and when to use, single-agent vs. multi-agent designs for AI systems.
- Understood the 4-step incident investigation workflow (Triage – Investigation – Mitigation – Improvement) of a DevOps AI agent.
- Learned the principles for setting up a private connection between an AI agent and an MCP server using a VPC Endpoint, an ALB, and Route 53 Resolver.

### Applying to Work

- **Apply a specialized multi-agent model** per role to optimize cost and control access scope.
- **Design Voice AI or chatbots with Vietnamese-specific considerations** (speaker gender, interruption handling, regional accents) instead of directly reusing international models.
- **Reference the DevOps agent model** to propose partial automation of incident investigation work in my own systems.
- **Apply private-first security principles** (VPC Endpoint, no public endpoint) when integrating an AI agent with internal enterprise systems.

### Event Experience

Attending FCAJ Community Day in June 2026 was a chance to catch up on the latest AI agent applications in cloud, while watching several hands-on demos from real companies. Some highlights:

#### Learning from speakers with hands-on experience

- Speakers from a range of companies brought diverse perspectives, from startups to solutions built for banks and large corporations.
- Had the chance to ask questions directly and get concrete feedback on deploying AI agents in production environments.

#### Hands-on demo experience

- Watched several live demos: a Voice Agent answering questions by voice, a DevOps Agent auto-investigating a simulated DDoS incident, and Amazon Quick analyzing and scoring candidate CVs in real time.
- Got a clearer picture of the real-world operating cost of AI agent solutions on AWS through a Q&A on pricing.

#### Some event photos

![Photo evidence from Event 3](/images/4-EventParticipated/4.3-Event3/event3-photo1.jpg)
