---
title: "Blog 1"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---


# Guide your Amazon Aurora MySQL migration with Kiro powers

The blog introduces the new Amazon Aurora MySQL power for Kiro — an AI tool integrated into the IDE that helps automate and simplify the entire database migration process from Amazon RDS for MySQL to Amazon Aurora MySQL through 4 phases (Assess, Migrate, Promote, Switch) using natural language, minimizing preparation time and reducing cutover downtime to just tens of seconds.

Key points to know:

* Concept of Kiro Powers: An extension tool that equips Kiro IDE's AI with deep domain expertise in a specific technology (best practices, APIs, standard configurations).
* Composed of 3 Components: MCP servers (connect directly to read AWS/DB status), Steering files (encode expert best practices), and Validation hooks (check for errors prior to execution).
* 4-Phase Migration Process (Near-Zero Downtime): Assess $\rightarrow$ Migrate $\rightarrow$ Promote $\rightarrow$ Switch.
* Source Version Requirements: The source RDS MySQL instance must be running version 5.7.44+ or 8.0.28+.
* Storage Engine: Only InnoDB is supported. Any tables using MyISAM on the source DB must be converted to InnoDB before migration.
* Backup & Binlog: The source instance must have automated backups enabled with a retention period of at least 1 day (to enable binary logging).
* Scope: Currently supports migration only within the same AWS account and same Region.
* Safety Mechanism: The AI only recommends and generates commands; the system never modifies AWS resources without explicit human approval at each step.
* Post-Migration Capabilities: After a successful migration to Aurora, the AI can continue to assist with adding read replicas tailored to workloads, configuring Aurora Global Database (multi-Region for disaster recovery), schema design, and SQL query optimization.

This feature is particularly useful as it introduces a production-ready solution that transforms a complex and risky database migration from Amazon RDS to Aurora into automated steps via natural language. This AI tool not only minimizes downtime to tens of seconds but also ensures technical safety through its ability to automatically audit compatibility issues (such as binlog and InnoDB) against AWS expert best practices.

![](/images/blog1.png)

Link: https://www.facebook.com/groups/awsstudygroupfcj/permalink/2208778813220412/
