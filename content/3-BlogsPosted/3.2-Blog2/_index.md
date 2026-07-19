---
title: "Blog 2"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 3.2. </b> "
---

# OPTIMIZING AWS LAMBDA COSTS

The Serverless model on AWS Lambda is generally considered cost-effective thanks to its pay-per-use pricing. However, without proactive optimization, the end-of-month bill can far exceed expectations — costs silently accumulate from misconfigured memory sizes, poorly handled cold starts, or logs retained indefinitely.

Below are the most noteworthy AWS Lambda cost optimization strategies, summarized from a CloudKeeper article:

**Key optimization strategies**

* **Switch to AWS Graviton2 (ARM)**: The simplest yet most effective move. Roughly 20% cheaper than the x86 architecture, while performance is even better for many workloads.
* **Memory optimization (Right-sizing)**: Do not pick a memory size by guesswork. Use a tool such as AWS Lambda Power Tuning to find the balance point between RAM and CPU — more RAM means more CPU, shorter execution time, and lower total cost.
* **Handle Cold Start & Concurrency**: Configure a reasonable amount of Provisioned Concurrency to protect user experience without wasting reserved resources.
* **Clean up Logs**: CloudWatch Logs retained indefinitely are an often-overlooked cost trap. Set an appropriate Retention period for each log group.

The AWS Lambda Power Tuning results below clearly illustrate the balance point between cost and execution time: Best Cost at 512MB, Best Time at 2048MB, with Graviton consistently cheaper than x86:

![AWS Lambda Power Tuning Results](/images/3-BlogsPosted/3.2-Blog2/LambdaPowerTuningResults.jpg)

Full article for readers who want to dive deeper: [Reducing AWS Lambda Costs — Optimization Tips for Serverless Computing](https://www.cloudkeeper.com/insights/blog/reducing-aws-lambda-costs-optimization-tips-serverless-computing#toc-using-aws-graviton2)

[Link to the original post](https://www.facebook.com/share/p/1cMX6QhMWE/)
