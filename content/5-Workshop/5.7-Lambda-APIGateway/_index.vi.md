---
title : "Deploy backend lên Lambda + API Gateway"
date : 2024-01-01
weight : 7
chapter : false
pre : " <b> 5.7 </b> "
---

### Mục tiêu

Đây là bước quan trọng nhất workshop: chúng ta sẽ đóng gói backend Express thành file zip, tạo Lambda function `phim-backend` (handler `src/lambda.js`), rồi mở ra internet qua **API Gateway HTTP API** với route `ANY /{proxy+}` — biến nguyên một app Express thành một endpoint serverless mà không cần viết lại route nào.

### 7.1. Đóng gói source code

Chạy PowerShell trong thư mục `phim-be/`:

```powershell
# 1. Cài dependencies production (bỏ devDependencies)
npm ci --omit=dev
```

![npm ci production](/images/5-Workshop/5.7-Lambda-APIGateway/01-npm-ci-production.png)

```powershell
# 2. Cài binary sharp cho Linux (Lambda chạy Linux, máy bạn là Windows)
npm install --os=linux --cpu=x64 sharp
```

![cài sharp cho linux](/images/5-Workshop/5.7-Lambda-APIGateway/02-install-sharp-linux.png)

```powershell
# 3. Nén source + node_modules
Compress-Archive -Path src,node_modules,package.json -DestinationPath phim-be-lambda.zip -Force

# 4. Xem kích thước file zip
(Get-Item phim-be-lambda.zip).Length / 1MB
```

![zip sẵn sàng](/images/5-Workshop/5.7-Lambda-APIGateway/03-zip-ready.png)

{{% notice tip %}}
Nếu zip **> 50MB** thì không upload trực tiếp được trên Console — đẩy qua S3 (dùng luôn bucket bước 4):

```powershell
aws s3 cp phim-be-lambda.zip s3://phim-avatars-<ACCOUNT_ID>/deploy/phim-be-lambda.zip
```
{{% /notice %}}

### 7.2. Tạo Lambda function `phim-backend`

1. Console → **Lambda** → **Create function** → **Author from scratch**:
   - **Function name:** `phim-backend`
   - **Runtime:** **Node.js 20.x** · **Architecture:** `x86_64`
   - **Permissions** → Change default execution role → **Use an existing role** → chọn **`phim-lambda-role`**
2. → **Create function**.

![tạo function](/images/5-Workshop/5.7-Lambda-APIGateway/04-create-function.png)

3. Upload code: tab **Code** → **Upload from** → **.zip file** (hoặc **Amazon S3 location** nếu zip > 50MB, dán `s3://phim-avatars-<ACCOUNT_ID>/deploy/phim-be-lambda.zip`).

![upload zip](/images/5-Workshop/5.7-Lambda-APIGateway/05-upload-zip.png)

4. **Runtime settings** → **Edit** → **Handler:** `src/lambda.handler` → Save.
5. **Configuration → General configuration → Edit:** Memory **1024 MB**, Timeout **30 giây** → Save.

![general configuration](/images/5-Workshop/5.7-Lambda-APIGateway/06-general-configuration.png)

6. **Configuration → Environment variables → Edit**, thêm:

| Key | Value |
|---|---|
| `USE_SSM` | `true` |
| `SSM_PREFIX` | `/phim/prod` |
| `STORAGE_DRIVER` | `s3` |
| `S3_AVATAR_BUCKET` | `phim-avatars-<ACCOUNT_ID>` |
| `NODE_ENV` | `production` |

![environment variables](/images/5-Workshop/5.7-Lambda-APIGateway/07-environment-variables.png)

### 7.3. Test function trong Console

Tab **Test** → **Create new event** → Event name `health-check` → dán event API Gateway v2:

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

→ **Test**. Kết quả mong đợi: `statusCode: 200`, body chứa `{"status":"ok","message":"Server is running"}`.

![test lambda](/images/5-Workshop/5.7-Lambda-APIGateway/08-test-lambda.png)

### 7.4. Tạo API Gateway HTTP API

1. Console → **API Gateway** → **Create API** → khung **HTTP API** → **Build**:
   - **Integrations:** Add integration → **Lambda** → chọn `phim-backend`
   - **API name:** `phim-api`

![configure api](/images/5-Workshop/5.7-Lambda-APIGateway/09-configure-api.png)

2. **Configure routes:** Method **ANY** · Resource path **`/{proxy+}`** · Integration target `phim-backend`.

![configure routes](/images/5-Workshop/5.7-Lambda-APIGateway/10-configure-routes.png)

3. **Stages:** giữ `$default`, **Auto-deploy = ON** → **Next**.

![define stages](/images/5-Workshop/5.7-Lambda-APIGateway/11-define-stages.png)

4. → **Create**. Copy **Invoke URL** (dạng `https://<api-id>.execute-api.ap-southeast-1.amazonaws.com`).

![tạo api](/images/5-Workshop/5.7-Lambda-APIGateway/12-create-api.png)

### 7.5. Kiểm tra từ internet

```powershell
curl https://<api-id>.execute-api.ap-southeast-1.amazonaws.com/api/health-check
# {"status":"ok","message":"Server is running"}

curl "https://<api-id>.execute-api.ap-southeast-1.amazonaws.com/api/movies?limit=2"
# JSON danh sách phim
```

![kiểm tra api curl](/images/5-Workshop/5.7-Lambda-APIGateway/13-check-api-curl.png)

### Kết quả mong đợi

- Lambda `phim-backend` test nội bộ trả 200.
- Invoke URL công khai trả đúng dữ liệu qua HTTPS.

### 🛠 Troubleshooting

| Lỗi | Xử lý |
|---|---|
| Test trả `Cannot find module 'sharp'` / lỗi binary | Thiếu bước cài sharp cho Linux — chạy lại 7.1 mục 2 rồi nén, upload lại |
| `Task timed out after 3.00 seconds` | Chưa tăng Timeout lên 30s (bước 7.2.5) |
| 500 + log `MongooseServerSelectionError` | Sai `MONGODB_URI` trong SSM, hoặc Atlas chưa mở Network Access 0.0.0.0/0 |
| `AccessDeniedException` SSM trong log | Role thiếu policy `phim-app-access` hoặc sai `SSM_PREFIX` |
| Zip quá 250MB (unzipped) | Xóa `node_modules` rồi `npm ci --omit=dev` lại (đảm bảo không dính devDependencies) |
