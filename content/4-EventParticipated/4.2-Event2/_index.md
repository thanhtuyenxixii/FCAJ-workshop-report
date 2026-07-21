---
title: "Event 2"
date: 2026-05-23
weight: 2
chapter: false
pre: " <b> 4.2. </b> "
---

# Summary Report: “FCAJ Community Day – May 23, 2026”

### Event Objectives

- Master AI context engineering and the mindset for building a "Second AI Brain."
- Introduce Amazon Quick, a friendly assistant for every type of user.
- Introduce the BMAD Method to help control source code quality and reduce AI hallucination.

### Speakers

- **Anh Tinh** – Platform Engineer at GoTymeX
- **Anh Hai Anh** – G-AsiaPacific Vietnam, AWS Community Builder (Security)
- **Nguyen Tuan Thinh** – DevOps/Cloud Engineer at AWS
- **UTM Morpho project team** (Chi Uyen, Thao, Mai)
- **Anh Dao Duc** – Solution Architect at Cloud Kinetics
- **Chi Vy** – Senior Business Systems Analyst at VPBank

### Key Highlights

#### Context management and the mindset for building a Second AI Brain

- **Context Engineering:** Quality over quantity — provide clear goals, constraints, and specific standards instead of stuffing in raw data or repeating redundant information.
- **Systems thinking:** Shifting from one-off prompts to building systems with memory so AI can personalize and assist better over time.
- Applying the standard 4-element Context framework (Goal, Role, Format, Evidence) and leveraging note-taking tools (Obsidian) to build the Second AI Brain concept.

#### A friendly AI assistant architecture & a no-code data automation ecosystem with Amazon Quick

- **Complete agent model:** Combining an LLM with tool-connection protocols so AI acts as a friendly assistant, helping every user exploit and automate data with just chat commands.
- **Quick Chat/Quick Sight:** Lets users ask questions to analyze raw data and automatically generate report dashboards.
- **Quick Flows/Quick Spaces:** Automatically generates smart workflows and builds a shared knowledge space for teams without writing a single line of code.

#### Optimizing cloud cost and edge infrastructure security

- **New Flat-Rate Pricing mechanism:** A solution that completely eliminates the risk of a runaway bill after an overnight DDoS under a pay-as-you-go model. Businesses pay a fixed-cost package with no extra charges (if usage is exceeded, the system is simply throttled).
- **Mutual TLS:** Two-way authentication handshake between client and server for secure financial applications.
- **VPC Origin:** Creates a direct tunnel from CloudFront into a private subnet, fully hiding the server infrastructure from the public internet.

#### Product development process from real hackathon experience

- **Finding the idea:** Solving the problem where AI keeps having to regenerate the entire UI from scratch just for a small color or spacing tweak, wasting tokens and time.
- **The UTM Morpho solution:** Building an app that uses AI to generate UIs from different templates that can then be edited directly on the UI itself.
- **Lessons from 36 hours under pressure:** How to manage token limits, handle AI over-generation errors, pick out the core features and cut the excess ones when overwhelmed with ideas, split work by each member's strengths, and manage stamina to avoid burnout.

#### The math behind LLMs & strategies for taming their nondeterminism

- Even at Temperature = 0, cloud LLMs still produce inconsistent results due to GPU floating-point rounding errors and commercial inference batching. Only a fully self-hosted local model is 100% identical every time.
- **Solution:** Set Temp = 0.1, enable JSON mode, design fault-tolerant downstream logic, and test continuously.

#### Multi-agent applications and enterprise compliance

- **Business problem:** Building a Virtual Credit Committee model to score startup creditworthiness using multidimensional alternative data.
- **System architecture:** A coordinating Manager agent breaks tasks down to specialized sub-agents (Financial Analyst, Market Analyst, Risk Assessor) that cross-challenge each other.
- **Compliance:** Strictly managing the boundaries of AI autonomy, defending against MCP protocol attacks, and enforcing clear audit trails.

### Key Takeaways

#### Architecture & Technology

- How to break large problems down into specialized agents to overcome context-window limits.
- Understanding edge network security architecture and the root cause of cloud LLM result inconsistency.

#### Systems Thinking

- A deeper understanding of edge network security architecture (VPC Origin, mTLS, multi-tier caching) to protect enterprise-grade systems.

### Applying to Work

- **Rigorous prompt engineering:** Applying the 4-element Context framework (Goal, Role, Format, Evidence). Always break problems down into clear sub-problems before handing them to AI.
- **Optimizing workflows:** Using natural-language tools to quickly automate raw-data workflows and generate report dashboards without code.
- **Infrastructure operations:** Paying attention to manually configuring the Flat-Rate Pricing package on the CloudFront console to proactively manage cost risk for projects.

### Event Experience

Attending the Meetup offered many practical perspectives on career direction in the GenAI era, along with concrete solutions and mindsets for mastering technology. Some highlights:

#### Hands-on with visual tools

- Watched a live demo of the **Amazon Quick** ecosystem (Quick Chat, Quick Flows, Quick Sight) - a visual, fully natural-language solution for quickly automating raw-data workflows and generating instant report dashboards on AWS.
- Explored the serverless Multi-Agent chain solution through the real **UTM Morpho** project - a complete solution for converting design mockups into HTML/CSS code without AI having to regenerate the entire interface every time a small detail needs editing.

#### Lessons Learned

- Proactively controlling and managing knowledge through a solid systems-design mindset (Second AI Brain), combined with a strong grasp of edge infrastructure security (mTLS, VPC Origin, Flat-Rate Pricing).
- Enterprise reality always comes first: large enterprise systems can't just be a thin AI wrapper — edge infrastructure safety, legal compliance boundaries, and audit-trail capability must come first.

#### Some event photos

![Photo evidence from Event 2](/images/4-EventParticipated/4.2-Event2/event2-photo1.jpg)
