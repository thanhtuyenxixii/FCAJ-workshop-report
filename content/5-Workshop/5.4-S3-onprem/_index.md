---
title : "Access S3 from on-premises"
date : 2024-01-01
weight : 4
chapter : false
pre : " <b> 5.4. </b> "
---

In this section, we will compile the React/Vite frontend application, inject the configuration variables outputted by AWS CloudFormation, and upload the static distribution assets to Amazon S3 to be distributed globally via Amazon CloudFront CDN.

#### 1. Manual Deployment Process (For Reference)

Usually, deploying a Single Page Application (SPA) to AWS involves:
1. Retrieving CloudFormation stack outputs.
2. Generating a local frontend/.env file.
3. Running npm install and npm run build to generate compiled static assets in frontend/dist/.
4. Uploading assets to S3 website bucket:
   ```bash
   aws s3 sync frontend/dist/ s3://<Frontend-S3-Bucket> --delete
   ```
5. Invalidating CloudFront cache so client web browsers fetch the new index page:
   ```bash
   aws cloudfront create-invalidation --distribution-id <CloudFront-Distribution-ID> --paths "/*"
   ```

---

#### 2. Automated Deployment Using PowerShell Script

To speed up the process and prevent human configuration errors, we provide a pre-made deployment script **`deploy-frontend.ps1`** in the root workspace directory.

This script fetches CloudFormation outputs, writes the configuration .env file, installs dependencies, builds the Vite production application, and uploads it to S3, followed by a CDN cache invalidation.

##### Steps:

1. Open a PowerShell terminal.
2. Run the deployment script:

```powershell
powershell -ExecutionPolicy Bypass -File .\deploy-frontend.ps1
```

![Deploy script execution console](/images/5-Workshop/5.4-S3-onprem/Deploy-frontend.png)

![Deploy script execution console](/images/5-Workshop/5.4-S3-onprem/Deploy-frontend-2.png)
##### Output Console:

```text
=== Step 1: Fetching AWS CloudFormation Stack Outputs ===
API URL: https://ki73ac3b15.execute-api.ap-southeast-1.amazonaws.com/Prod/
Cognito UserPool: ap-southeast-1_FMpyd6sHf
Cognito Client ID: 7tlipc9j4vdt7lvd5rh3f5pkq0
Target S3 Bucket: essay-scoring-frontend-919968175298-ap-southeast-1
CloudFront URL: https://d1bzf841axvekw.cloudfront.net/

=== Step 2: Creating Frontend Production Environment Config ===
Config saved to frontend\.env

=== Step 3: Building React Application ===
vite v8.1.3 building client environment for production...
built in 713ms

=== Step 4: Deploying Assets to S3 Bucket ===
upload: frontend\dist\assets\index-Bc35BIHX.js to s3://...

=== Step 5: Invalidating CloudFront Cache ===
Cache invalidation request submitted for Distribution: E1CXVT41RVE5J8

=== Deployment Completed Successfully! ===
Your application is live at: https://d1bzf841axvekw.cloudfront.net/
```

Once the **Deployment Completed Successfully!** message is printed, your web app is fully deployed! Open the returned **CloudFront URL** in your browser to access the website.



