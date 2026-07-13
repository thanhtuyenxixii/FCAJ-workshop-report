---
title: "Workshop"
date: 2024-01-01
weight: 5
chapter: false
pre: " <b> 5. </b> "
---

# Automated English Essay Scoring System (AWS Serverless & Gemini AI)

#### Overview

In this hands-on workshop, you will build and deploy a fully automated **English Essay Scoring and Feedback System** using serverless architecture on AWS and generative AI with the Google Gemini API.

The system allows students or teachers to log in, upload essays as plain text `.txt` files, photos of handwritten/printed essays (`.png`, `.jpg`, `.jpeg`), or `.pdf` documents. The system automatically performs OCR via Amazon Textract, grades the essay via Gemini AI, stores the results in DynamoDB/S3, and sends score notifications via email (Amazon SNS).

#### Content

1. [Workshop Overview](5.1-Workshop-overview/)
2. [Prerequisites](5.2-Prerequiste/)
3. [Deploy Backend (SAM & Step Functions)](5.3-Deploy-backend/)
4. [Deploy Frontend (CloudFront & S3 website)](5.4-Deploy-frontend/)
5. [Testing & Verification (Textract & SNS Email)](5.5-Test-verify/)
6. [Cleanup Resources](5.6-Cleanup/)