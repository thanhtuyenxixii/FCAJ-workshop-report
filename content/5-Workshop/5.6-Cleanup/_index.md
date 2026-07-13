---
title : "Clean up"
date : 2024-01-01
weight : 6
chapter : false
pre : " <b> 5.6. </b> "
---
To avoid incurring unexpected charges on your AWS account after finishing the workshop, clean up the resources by following the steps below.

> [!CAUTION]
> CloudFormation **cannot delete S3 Buckets that are not empty**. You must empty all S3 buckets first before deleting the stack.

---

#### Step 1: Empty S3 Buckets

Use AWS CLI to delete all objects in the three buckets:

```bash
# 1. Empty the raw uploads bucket
aws s3 rm s3://essay-scoring-raw-919968175298-ap-southeast-1 --recursive

# 2. Empty the AI evaluation results bucket
aws s3 rm s3://essay-scoring-result-919968175298-ap-southeast-1 --recursive

# 3. Empty the frontend distribution bucket
aws s3 rm s3://essay-scoring-frontend-919968175298-ap-southeast-1 --recursive
```

*(Note: Replace with your actual S3 bucket names from the CloudFormation Outputs).*

---

#### Step 2: Delete the SAM Stack

Once the S3 buckets are empty, delete the CloudFormation stack:

```bash
sam delete
```

Confirm by typing `y` (Yes) when prompted. This removes the API Gateway, Lambdas, Cognito User Pool, DynamoDB Table, CloudFront distribution, and associated IAM roles.

Alternatively, you can visit the [AWS CloudFormation Console](https://ap-southeast-1.console.aws.amazon.com/cloudformation/home?region=ap-southeast-1), select the **`essay-scoring`** stack, and click **Delete**.

---

#### Step 3: Delete the Gemini API Key from SSM Parameter Store

Run this command to delete the stored secure parameter:

```bash
aws ssm delete-parameter --name "/essay-scoring/gemini-api-key"
```