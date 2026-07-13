---
title : "Access S3 from VPC"
date : 2024-01-01
weight : 3
chapter : false
pre : " <b> 5.3. </b> "
---

In this section, we will build and deploy all backend serverless resources to AWS using the **AWS SAM CLI**.

#### 1. Backend Resource Architecture

The backend code is located inside the `backend/` directory:
* **`template.yaml`:** Defines infrastructure including S3, DynamoDB, API Gateway, Lambda functions, SNS, Cognito, CloudFront, and Step Functions.
* **`state-machine.asl.json`:** Defines the Step Functions state machine logic for processing essays (type validation -> Textract OCR -> AI grading -> saving results -> sending email notifications).
* **Lambda Functions:**
  * `GetPresignedUrlHandler` (Java): Generates S3 Presigned URLs for direct client uploads.
  * `RouterStoreHandler` (Java): Handles route APIs, logs database records, and sends SQS messages.
  * `JobStarterHandler` (Java): Triggers Step Functions state machine execution asynchronously via SQS.
  * `textract-parser` (Node.js): Parses Textract OCR blocks into raw paragraph text.
  * `AiEvaluatorHandler` (Java): Retreives Gemini API Key from SSM, sends essay content to Gemini, and stores JSON result reports on S3.

---

#### 2. Build the Application

From the root project directory, run the SAM build command to compile Java handlers and Node dependencies:

```bash
sam build
```

Once completed successfully, you should see the `Build Succeeded` message.
![SAM Build Thành công](/images/5-Workshop/5.3-S3-vpc/sam-build.jpg)

---

#### 3. Deploy the Stack to AWS

Deploy the stack by running the following command (replace `testuser@gmail.com` with your actual email to receive notifications):

```bash
sam deploy \
    --stack-name essay-scoring \
    --region ap-southeast-1 \
    --parameter-overrides GeminiApiKeyParam=/essay-scoring/gemini-api-key NotificationEmail=testuser@gmail.com \
    --no-confirm-changeset \
    --resolve-s3 \
    --capabilities CAPABILITY_IAM
```

##### Command Parameter Breakdown:
* --stack-name: The CloudFormation stack name is essay-scoring.
* --region: We deploy in ap-southeast-1 (Singapore).
* --parameter-overrides: Passes stack parameter configs:
  * GeminiApiKeyParam: Points to the SSM secure parameter created in the prerequisites.
  * NotificationEmail: The target email for SNS notifications.
* --resolve-s3: Allows SAM to resolve code packaging buckets automatically.
* --capabilities CAPABILITY_IAM: Authorizes SAM to create IAM Roles for functions and the state machine.

---

#### 4. Save CloudFormation Outputs

When deployment finishes, keep a record of the values in the **Outputs** section. They are needed to configure the React frontend:

* **`ApiUrl`:** API Gateway REST endpoint.
* **`CognitoUserPoolId`:** Cognito User Pool ID.
* **`CognitoUserPoolClientId`:** Cognito Web Client ID.
* **`FrontendUrl`:** CloudFront CDN Domain distributing the website.

![Màn hình CloudFormation Outputs](/images/5-Workshop/5.3-S3-vpc/CloudFormation.png)