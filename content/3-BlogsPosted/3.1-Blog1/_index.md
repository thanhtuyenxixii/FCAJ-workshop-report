---
title: "Blog 1"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---
# AWS LAMBDA DURABLE FUNCTIONS

AWS Lambda's Serverless architecture has a well-known limitation: statelessness. Each Invoke is an independent execution lifecycle, capped at 15 minutes; if the function fails partway through, the entire process is lost and must be restarted from the beginning.

This limitation makes it difficult to build multi-step workflows (for example, order processing consisting of Validate ➔ Charge payment ➔ Wait for webhook ➔ Send email) or processes that require manual approval over hours or days. To work around it, development teams typically combine Step Functions, SQS, Cron Jobs, DynamoDB, and similar services, which adds unnecessary complexity to the system.

At AWS re:Invent 2025, AWS introduced a new feature designed to address this problem: AWS Lambda Durable Functions (Session CNS380, presented by Eric Johnson and Michael Gasch).

**What is AWS Lambda Durable Functions?**

This feature allows a Lambda execution to automatically checkpoint its progress and pause (wait) without incurring compute costs, then automatically resume from the exact point it left off once a signal is received. The lifetime of an Execution can now extend up to 1 year, while the limit for each individual Invoke remains 15 minutes. When a function is paused or encounters an error, Lambda re-invokes it from the start, but replays the steps already completed based on results saved in the execution log, and only executes the remaining, unfinished portion of the work.

![AWS Lambda Durable Functions](/images/3-BlogsPosted/3.1-Blog1/AWSLambdaDurableFunctions.drawio.png)

[Link to the original post](https://www.facebook.com/groups/awsstudygroupfcj/permalink/2206391430125817/?rdid=ygFv7ftHySKZbKkF#)

...Guide...
