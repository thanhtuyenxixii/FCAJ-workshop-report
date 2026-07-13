---
title: "Proposal"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 2. </b> "
---





# Automated English Essay Scoring System

### 1. Executive Summary
The Automated English Essay Scoring System is an automated English essay assessment system built on the AWS Cloud platform and integrated with the Google Gemini API to provide fast, accurate, and consistent essay evaluation. The system allows users to upload essays, automatically extracts the content, analyzes it using artificial intelligence, and returns scores along with detailed feedback.

### 2. Problem Statement

*Current Problem*  
English essay grading is primarily performed manually, which is time-consuming, costly, and highly dependent on human graders, resulting in inconsistent evaluation outcomes. In addition, learners often have to wait a long time to receive their scores and detailed feedback.

*Solution*  
Develop an automated English essay scoring system on the AWS platform integrated with the Google Gemini API to analyze essay content, evaluate writing quality, and provide fast and accurate scores along with detailed feedback.

*Benefits and Return on Investment (ROI)*  
The system reduces grading time and operational costs, improves consistency in evaluation, and enhances learning efficiency. Leveraging a serverless architecture on AWS also optimizes operational costs, enables scalability, and delivers long-term return on investment.

### 3. Solution Architecture
The system is built using a serverless architecture on the AWS platform. Users access the application through AWS Amplify and Amazon CloudFront, then send requests to Amazon API Gateway. Amazon Cognito is used for user authentication, AWS Lambda handles business logic, Amazon S3 stores essays and processing results, while Amazon SQS and AWS Step Functions orchestrate the processing workflow. Amazon Textract extracts text from documents, and the Google Gemini API evaluates and scores the essays. The results are stored in Amazon DynamoDB and Amazon S3, while Amazon SNS sends notifications to users. The entire system is monitored using Amazon CloudWatch and AWS X-Ray, with access management handled by AWS IAM.

![](/images/2-Proposal/AWS.jpeg.png)

### AWS Services Used
| Component Zone | AWS Service Used | Precise Technical Role |
| :--- | :--- | :--- |
| **Edge / Frontend** | **AWS Amplify + CloudFront** | Hosts and distributes the web frontend application globally with low latency. |
| **Authentication & API** | **Amazon Cognito + API Gateway** | Validates JWT tokens and provides a secure endpoint to fetch S3 Presigned URLs. |
| **Document Ingestion** | **Amazon S3 + Amazon SQS** | Ingests raw essay payloads into a secure bucket and queues event notifications to buffer compute. |
| **AI Core Orchestration** | **AWS Step Functions** | Drives the main state machine workflow, managing system forks, status tracking, and error handling. |
| **Document Parsing** | **Amazon Textract** | Executes advanced OCR to extract raw text blocks from submitted images or PDF scans. |
| **AI Assessment** | **AWS Lambda + Gemini API** | Invokes `Gemini 1.5 Flash` to evaluate text quality, structural grammar, and generate structured score JSONs. |
| **Data Persistence** | **Amazon DynamoDB + Amazon S3** | Persists execution states & high-level scores in NoSQL; stores granular JSON reports in a secure storage bucket. |
| **Notification Engine** | **Amazon SNS** | Dispatches instant evaluation updates and final metrics (Email/SMS) directly back to users/admins. |

### 4. Technical Implementation

*Implementation Phases*

1. *System Analysis and Design*: Analyze system requirements, design the system architecture, database, user interface, and select appropriate AWS services.
2. *System Development*: Develop the user interface, backend services, integrate AWS services (Amplify, API Gateway, Lambda, Cognito, S3, DynamoDB, etc.), and integrate the Google Gemini API for essay scoring.
3. *Testing*: Test system functionality, fix defects, optimize performance, verify security, evaluate scoring results, and finalize the system.
4. *Deployment*: Deploy the system on AWS, verify production operation, evaluate outcomes, complete documentation, and prepare the final report and presentation.

### 5. Roadmap & Milestones

#### Cross-cutting Security, Governance & Observability
- **Keywords**: *AWS IAM, Amazon X-Ray, Amazon CloudWatch, AWS Systems Manager*.
- **Explanation**: 
    * **AWS IAM**: Enforces least-privilege role permissions across all serverless compute resources and service-to-service interactions.
    * **Amazon CloudWatch**: Collects pipeline runtime logs, execution metrics, and triggers alarms on system errors or high failure rates.
    * **Amazon X-Ray**: Provides end-to-end distributed tracing across API Gateway, Lambda, and Step Functions to isolate bottleneck latencies.
    * **AWS Systems Manager**: Securely stores environment variables and external API credentials (such as the Google Gemini API Key) within Parameter Store.

### 6. Budget Estimation

*Infrastructure Costs*
- AWS Amplify: USD 2.50/month (120 build minutes, 2 GB storage, 30 GB data transfer).
- Amazon CloudFront: USD 1.20/month (30 GB outbound data transfer, 100,000 HTTPS requests).
- Amazon API Gateway: USD 0.10/month (15,000 REST API requests).
- Amazon Cognito: USD 0.00/month (300 Monthly Active Users – within the Free Tier).
- AWS Lambda: USD 0.25/month (12,000 requests, 512 MB memory, average execution time of 2 seconds).
- Amazon S3 Standard: USD 0.60/month (20 GB storage, 6,000 PUT requests, 10,000 GET requests).
- Amazon SQS: USD 0.01/month (6,000 message requests).
- Amazon Textract: USD 9.00/month (6,000 processed document pages).
- Amazon DynamoDB: USD 0.80/month (2 GB storage, 20,000 read requests, 10,000 write requests).
- Amazon SNS: USD 0.01/month (3,000 notifications sent).
- Amazon CloudWatch: USD 1.20/month (5 GB logs, 20 metrics, 1 dashboard).

*Total*: **USD 15.67/month**, **USD 188.04/year**

### 7. Risk Assessment

*Risk Matrix*
- Internet connectivity loss.
- Project delays due to workload.
- AWS service costs exceeding the estimated budget.
- Integration issues between AWS services and the Google Gemini API.
- AI-generated essay evaluations may be inaccurate or inconsistent.
- Limited experience with certain AWS services.

*Mitigation Strategies*
- Internet connectivity: Store data locally on Raspberry Pi using Docker.
- Project delays: Create a detailed weekly schedule and prioritize core features.
- AWS cost overrun: Use AWS Pricing Calculator, monitor the Billing Dashboard, and leverage the AWS Free Tier whenever possible.
- AWS and Gemini API integration issues: Test each component individually before full system integration.
- AI evaluation inaccuracies: Design effective prompts, test with multiple datasets, and refine the evaluation process.
- Limited AWS experience: Study AWS documentation, practice in a testing environment, and refer to official AWS resources.

*Contingency Plan*
- Prioritize core features, continuously monitor costs, and perform regular data backups to minimize risks. In case of failures, the system will recover data, retry failed requests, or roll back to the latest stable version to ensure continuous operation.

### 8. Expected Outcomes

#### Technical Improvements
* **Automated Scalability**: Replaces manual, human-driven essay grading with a hands-off, 24/7 automated engine capable of scaling dynamically to handle peaks of thousands of submissions without server degradation.
* **Format-Agnostic Processing**: Establishes a highly resilient document processing channel that transparently standardizes handwriting scans, digital PDFs, and loose plain text into a clean tokenized context.
* **Deterministic Structured Data**: Converts unstructured, variable-length compositions into strict, machine-readable JSON schemas containing granular band score breakdowns for downstream application use.

#### Long-Term Value
* **Academic Data Foundation**: Compiles a centralized, secure, historical database of student submissions and structured scoring logs, creating an open research catalog for future NLP and customized LLM model fine-tuning within the lab.
* **Educational Asset & Template**: Serves as a production-ready, pure serverless blueprint for other engineering teams to study asynchronous data-buffering, secure edge integration, and cost-optimized orchestration.