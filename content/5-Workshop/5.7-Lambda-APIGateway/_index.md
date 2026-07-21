---
title : "Deploy the backend to Lambda + API Gateway"
date : 2024-01-01
weight : 7
chapter : false
pre : " <b> 5.7 </b> "
---

### Goal

Package the Express backend into a zip, create the `phim-backend` Lambda function (handler `src/lambda.js`), and expose it via an **API Gateway HTTP API** with an `ANY /{proxy+}` route.

### 7.1. Package the source

In PowerShell inside `phim-be/`:

```powershell
# 1. Production dependencies only (skip devDependencies)
npm ci --omit=dev
```

![npm ci production](/images/5-Workshop/5.7-Lambda-APIGateway/01-npm-ci-production.png)

```powershell
# 2. Install sharp's Linux binaries (Lambda runs Linux, your machine is Windows)
npm install --os=linux --cpu=x64 sharp
```

![install sharp linux](/images/5-Workshop/5.7-Lambda-APIGateway/02-install-sharp-linux.png)

```powershell
# 3. Zip source + node_modules
Compress-Archive -Path src,node_modules,package.json -DestinationPath phim-be-lambda.zip -Force

# 4. Check the zip size
(Get-Item phim-be-lambda.zip).Length / 1MB
```

![zip ready](/images/5-Workshop/5.7-Lambda-APIGateway/03-zip-ready.png)

{{% notice tip %}}
If the zip is **> 50MB**, upload via S3 instead (reuse the bucket from step 4):

```powershell
aws s3 cp phim-be-lambda.zip s3://phim-avatars-<ACCOUNT_ID>/deploy/phim-be-lambda.zip
```
{{% /notice %}}

### 7.2. Create the `phim-backend` Lambda

1. Console → **Lambda** → **Create function** → **Author from scratch**: name `phim-backend`, runtime **Node.js 20.x**, arch `x86_64`, execution role → **Use an existing role** → `phim-lambda-role` → **Create function**.

![create function](/images/5-Workshop/5.7-Lambda-APIGateway/04-create-function.png)

2. **Code** tab → **Upload from** → **.zip file** (or **Amazon S3 location** if > 50MB, paste `s3://phim-avatars-<ACCOUNT_ID>/deploy/phim-be-lambda.zip`).

![upload zip](/images/5-Workshop/5.7-Lambda-APIGateway/05-upload-zip.png)

3. **Runtime settings → Edit → Handler:** `src/lambda.handler` → Save.
4. **Configuration → General configuration → Edit:** Memory **1024 MB**, Timeout **30 seconds** → Save.

![general configuration](/images/5-Workshop/5.7-Lambda-APIGateway/06-general-configuration.png)

5. **Configuration → Environment variables → Edit**, add:

| Key | Value |
|---|---|
| `USE_SSM` | `true` |
| `SSM_PREFIX` | `/phim/prod` |
| `STORAGE_DRIVER` | `s3` |
| `S3_AVATAR_BUCKET` | `phim-avatars-<ACCOUNT_ID>` |
| `NODE_ENV` | `production` |

![environment variables](/images/5-Workshop/5.7-Lambda-APIGateway/07-environment-variables.png)

### 7.3. Test the function in the Console

**Test** tab → **Create new event** → name `health-check` → paste an API Gateway v2 event:

```json
{
  "version": "2.0",
  "routeKey": "ANY /{proxy+}",
  "rawPath": "/api/health-check",
  "rawQueryString": "",
  "headers": { "accept": "application/json" },
  "requestContext": {
    "http": { "method": "GET", "path": "/api/health-check", "protocol": "HTTP/1.1", "sourceIp": "1.1.1.1", "userAgent": "test" },
    "routeKey": "ANY /{proxy+}",
    "stage": "$default"
  },
  "isBase64Encoded": false
}
```

→ **Test**. Expect `statusCode: 200` and a body containing `{"status":"ok","message":"Server is running"}`.

![test lambda](/images/5-Workshop/5.7-Lambda-APIGateway/08-test-lambda.png)

### 7.4. Create the HTTP API

1. Console → **API Gateway** → **Create API** → **HTTP API** → **Build**: integration **Lambda** → `phim-backend`, API name `phim-api`.

![configure api](/images/5-Workshop/5.7-Lambda-APIGateway/09-configure-api.png)

2. **Configure routes:** Method **ANY** · Resource path **`/{proxy+}`** · Integration target `phim-backend`.

![configure routes](/images/5-Workshop/5.7-Lambda-APIGateway/10-configure-routes.png)

3. **Stages:** keep `$default`, **Auto-deploy = ON** → **Next**.

![define stages](/images/5-Workshop/5.7-Lambda-APIGateway/11-define-stages.png)

4. → **Create**. Copy the **Invoke URL** (`https://<api-id>.execute-api.ap-southeast-1.amazonaws.com`).

![create api](/images/5-Workshop/5.7-Lambda-APIGateway/12-create-api.png)

### 7.5. Verify from the internet

```powershell
curl https://<api-id>.execute-api.ap-southeast-1.amazonaws.com/api/health-check
# {"status":"ok","message":"Server is running"}

curl "https://<api-id>.execute-api.ap-southeast-1.amazonaws.com/api/movies?limit=2"
# JSON list of movies
```

![check api curl](/images/5-Workshop/5.7-Lambda-APIGateway/13-check-api-curl.png)

### ✅ Expected result

- The internal Lambda test returns 200.
- The public Invoke URL returns correct data over HTTPS.

### 🛠 Troubleshooting

| Error | Fix |
|---|---|
| `Cannot find module 'sharp'` / binary error | Re-run the Linux sharp install (7.1 step 2), re-zip, re-upload |
| `Task timed out after 3.00 seconds` | Timeout not raised to 30s |
| 500 + `MongooseServerSelectionError` in logs | Wrong `MONGODB_URI` in SSM, or Atlas Network Access not open |
| `AccessDeniedException` for SSM | Role missing `phim-app-access` or wrong `SSM_PREFIX` |
| Unzipped size > 250MB | Delete `node_modules`, re-run `npm ci --omit=dev` (no devDependencies) |
