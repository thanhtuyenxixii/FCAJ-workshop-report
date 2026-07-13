---
title : "Introduction"
date : 2024-01-01 
weight : 1 
chapter : false
pre : " <b> 5.1. </b> "
---

#### System Architecture

The system is built entirely on a **Serverless** architecture, which automatically scales, optimizes costs, and requires no server management.

![](/images/2-Proposal/AWS.jpeg.png)

#### Key Project Components

1. **Identity & Security (Stage 1):**
   * **AWS Cognito User Pool:** Manages account registrations, logins, and client JWT token verification.
   * **AWS IAM Role & Policies:** Enforces the principle of least privilege.
2. **API & Ingestion (Stage 2):**
   * **Amazon API Gateway:** Provides secure REST endpoints protected by Cognito.
   * **S3 Presigned URL:** Direct uploads from client browsers to S3, bypassing Lambda.
   * **Amazon SQS:** Decouples API responses from backend state machine launches.
3. **AI Pipeline (Stage 3):**
   * **AWS Step Functions:** Orchestrates the serverless workflow.
   * **Amazon Textract:** Scans handwritten or printed documents for OCR text extraction.
   * **Google Gemini AI:** Evaluates grammar, vocabulary, structure, and coherence out of 100 points.
4. **Storage & Notification (Stage 4):**
   * **Amazon DynamoDB:** Stores metadata, status, and summary scores.
   * **Amazon S3 (Result Bucket):** Stores verbose JSON feedback reports.
   * **Amazon SNS:** Delivers grading feedback directly to the user's email inbox.
5. **Distribution (Stage 5):**
   * **Amazon CloudFront & S3 Website:** Distributes the React single-page app over secure HTTPS globally.