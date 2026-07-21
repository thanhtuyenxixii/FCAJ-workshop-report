---
title: "Workshop"
date: 2024-01-01
weight: 5
chapter: false
pre: " <b> 5. </b> "
---

# Deploying a Movie Streaming Web App on AWS Serverless

#### Overview

This workshop deploys a complete **online movie streaming platform** (Netflix-clone) — a Node.js/Express backend and a Next.js 15 frontend — onto a fully **serverless AWS architecture**.

You will move the system off a single PaaS host and onto **Lambda + API Gateway** (backend), **Amplify** (frontend), fronted by a single **CloudFront** distribution with **WAF** attached, with avatars in **S3**, secrets in **SSM Parameter Store**, email via **SES**, a daily **EventBridge** cron job, and **CloudWatch + SNS** monitoring/alerting — all wired together with least-privilege **IAM**.

**13 AWS services used:** Lambda · API Gateway · Amplify · CloudFront · WAF · ACM · S3 · SES · SNS · EventBridge Scheduler · CloudWatch · SSM Parameter Store · IAM.

**Estimated cost:** ~$16–18/month (mostly within the free tier). Remember to run the [Clean-up](5.13-Cleanup/) step after your demo to avoid ongoing charges.

#### Content

1. [Introduction](5.1-Introduction/)
2. [Prerequisite](5.2-Prerequisite/)
3. [Create secrets with SSM Parameter Store](5.3-SSM-Parameters/)
4. [Create the S3 avatar bucket](5.4-S3-Bucket/)
5. [Configure email with Amazon SES](5.5-SES-Email/)
6. [Create the Lambda IAM Role](5.6-IAM-Role/)
7. [Deploy the backend to Lambda + API Gateway](5.7-Lambda-APIGateway/)
8. [Daily cron with EventBridge Scheduler](5.8-EventBridge-Cron/)
9. [Deploy the frontend with AWS Amplify](5.9-Amplify-Frontend/)
10. [CloudFront + ACM + WAF](5.10-CloudFront-WAF/)
11. [Monitoring & alerting with CloudWatch + SNS](5.11-Monitoring/)
12. [Testing & validation](5.12-Testing/)
13. [Clean up](5.13-Cleanup/)
