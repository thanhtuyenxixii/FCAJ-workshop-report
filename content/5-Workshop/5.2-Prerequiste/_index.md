---
title : "Prerequiste"
date : 2024-01-01 
weight : 2 
chapter : false
pre : " <b> 5.2. </b> "
---
o begin deploying the Essay Scorer AI system, you need to install several development tools locally and perform some security configuration in your AWS account.

#### 1. Install Development Tools (Local Tools)

Ensure the following tools are installed on your computer:

* **AWS CLI:** To configure access credentials to your AWS account.
  * Configure using: `aws configure` (enter Access Key ID, Secret Access Key, and default region as `ap-southeast-1`).
* **AWS SAM CLI:** The command-line tool for managing and deploying serverless applications.
  * Download and install from AWS documentation. Check with: `sam --version`.
* **JDK 17 & Maven:** Required to build the Java Lambda functions.
  * Check with: `java -version` and `mvn -version`.
* **Node.js (LTS):** Required for building/bundling the frontend and parser Lambda.
  * Check with: `node -v` and `npm -v`.

---

#### 2. Get Gemini API Key from Google AI Studio

The system uses the **Gemini 1.5 Flash** model for essay evaluation. Get a key by:

1. Visit [Google AI Studio](https://aistudio.google.com/).
2. Log in and click **Get API Key**.
3. Create a new key and copy its value.

![Get Gemini API Key](/images/5-Workshop/5.2-Prerequisite/gemini-api-key.png)

---

#### 3. Store Key Securely in AWS SSM Parameter Store

To avoid exposing the key in code, save it in the **AWS Systems Manager (SSM) Parameter Store** as a `SecureString`.

Run this command in your terminal (replace with your actual key):

``bash
aws ssm put-parameter \
    --name "/essay-scoring/gemini-api-key" \
    --value "YOUR_GEMINI_KEY" \
    --type "SecureString" \
    --overwrite
``

![Create Parameter Store in AWS](/images/5-Workshop/5.2-Prerequisite/Parameter.png)
![Create Parameter Store in AWS](/images/5-Workshop/5.2-Prerequisite/Parameter-2.png)

> **Important Note:** The parameter name `/essay-scoring/gemini-api-key` must match the one defined in the `template.yaml` file.

---

#### 4. Notification Email for SNS

Prepare an email address (e.g. `testuser@gmail.com`) to receive scores and feedbacks via Amazon SNS.