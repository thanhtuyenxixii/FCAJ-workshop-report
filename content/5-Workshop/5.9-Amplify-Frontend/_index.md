---
title : "Deploy the frontend with AWS Amplify"
date : 2024-01-01
weight : 9
chapter : false
pre : " <b> 5.9 </b> "
---

### Goal

Deploy `phim-fe` (Next.js 15) to **Amplify Hosting**, connected to GitHub for automatic CI/CD: every push triggers a build and deploy.

### 9.1. Prepare the repo

Ensure `phim-fe/` is pushed to GitHub and `amplify.yml` sits at the frontend root.

### 9.2. Create the Amplify app

1. Console → **AWS Amplify** → **Create new app** (Host web app).
2. Choose **GitHub** → authorize → pick the repo + branch (e.g. `main`). For a monorepo, check **My app is a monorepo** and enter `phim-fe`.

![deploy from git provider](/images/5-Workshop/5.9-Amplify-Frontend/01-deploy-from-git.png)

3. Amplify auto-detects **Next.js - SSR** and reads the existing `amplify.yml` — keep defaults.
4. **Advanced settings → Environment variables:**

| Key | Value | Note |
|---|---|---|
| `NEXT_PUBLIC_API_URL` | `https://<api-id>.execute-api.ap-southeast-1.amazonaws.com/api` | ⚠️ **The `/api` suffix is required** — the FE reads this in `src/config/API.js` |

![advanced settings env](/images/5-Workshop/5.9-Amplify-Frontend/02-advanced-settings-env.png)

{{% notice tip %}}
To double-check the variable name: open `phim-fe/src/config/API.js`, line 1: `export const API_URL = process.env.NEXT_PUBLIC_API_URL || 'http://localhost:5000/api'` — confirms the variable name and that the URL must include `/api`.
{{% /notice %}}

5. → **Save and deploy**.

### 9.3. Wait for the build & verify

1. Watch the pipeline: Provision → Build → Deploy (~5–10 minutes the first time).
2. Open `https://<branch>.<app-id>.amplifyapp.com` — the movie homepage renders and movie lists load (the FE reaches API Gateway).

![deploy success](/images/5-Workshop/5.9-Amplify-Frontend/03-deploy-success.png)

### 9.4. Update FRONTEND_URL for the backend

The backend uses `FRONTEND_URL` for CORS. In **SSM Parameter Store**, set `/phim/prod/FRONTEND_URL` = `https://<branch>.<app-id>.amplifyapp.com`, then redeploy the `phim-backend` Lambda so a fresh cold start reads the new value.

### Expected result

- Green Amplify build; the site is reachable at `*.amplifyapp.com`; the homepage loads movies from the API.

### 🛠 Troubleshooting

| Error | Fix |
|---|---|
| Build fails at `npm ci` | Check the build log; usually a Node version issue — Amplify defaults to Node 20 for Next.js 15 |
| Site loads but no movie data | `NEXT_PUBLIC_API_URL` missing `/api` or wrong Invoke URL; fix and **Redeploy this version** |
| CORS errors in DevTools | `/phim/prod/FRONTEND_URL` doesn't match the Amplify domain (step 9.4) |
