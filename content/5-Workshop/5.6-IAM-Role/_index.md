---
title : "Create the Lambda IAM Role"
date : 2024-01-01
weight : 6
chapter : false
pre : " <b> 5.6 </b> "
---

### Goal

Create the `phim-lambda-role` IAM role for both Lambda functions (backend + cron) following the **Principle of Least Privilege**: it may only write logs, read/write the `avatars/*` prefix of one specific bucket, and read the `/phim/prod` SSM prefix. Nothing more.

### Steps

1. Console → **IAM** → **Roles** → **Create role**.
2. **Trusted entity type:** AWS service · **Use case:** **Lambda** → **Next**.

![select trusted entity](/images/5-Workshop/5.6-IAM-Role/01-select-trusted-entity.png)

3. **Add permissions:** check **`AWSLambdaBasicExecutionRole`** (CloudWatch Logs) → **Next**.

![add permissions](/images/5-Workshop/5.6-IAM-Role/02-add-permissions.png)

4. **Role name:** `phim-lambda-role` → **Create role**.

![name review create](/images/5-Workshop/5.6-IAM-Role/03-name-review-create.png)

5. Open the role → **Permissions** → **Add permissions → Create inline policy** → **JSON** tab → paste (replace `<ACCOUNT_ID>`; adjust the bucket name if different):

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AvatarBucket",
      "Effect": "Allow",
      "Action": ["s3:PutObject", "s3:GetObject", "s3:DeleteObject"],
      "Resource": "arn:aws:s3:::phim-avatars-<ACCOUNT_ID>/avatars/*"
    },
    {
      "Sid": "ReadSecrets",
      "Effect": "Allow",
      "Action": ["ssm:GetParametersByPath"],
      "Resource": "arn:aws:ssm:ap-southeast-1:<ACCOUNT_ID>:parameter/phim/prod*"
    }
  ]
}
```

![inline policy json](/images/5-Workshop/5.6-IAM-Role/04-inline-policy-json.png)

6. **Policy name:** `phim-app-access` → **Create policy**.

![role created](/images/5-Workshop/5.6-IAM-Role/05-role-created.png)

### Least-privilege explanation (for the Security section of your report)

| Statement | Allows | Does NOT allow |
|---|---|---|
| `AvatarBucket` | Put/Get/Delete objects **only under** `phim-avatars-<ACCOUNT_ID>/avatars/*` | Touching other buckets, deleting the bucket, changing policies, listing all of S3 |
| `ReadSecrets` | Reading parameters **only under** `/phim/prod` | Other projects' secrets, writing/deleting parameters |
| `AWSLambdaBasicExecutionRole` | Creating log groups/streams, writing logs | Reading other services' logs |

No `ses:*` is granted — email uses dedicated SMTP credentials (step 5), one more layer of separation.

### Expected result

`phim-lambda-role` exists, trusts `lambda.amazonaws.com`, and has exactly the two policies shown.

### 🛠 Troubleshooting

| Error | Fix |
|---|---|
| Policy JSON fails validation | Check `<ACCOUNT_ID>` was replaced; ARNs must not contain spaces |
| Lambda later gets `AccessDenied` reading SSM | Wrong region in the ARN (`ap-southeast-1`) or wrong `/phim/prod` prefix |
| Lambda `AccessDenied` writing S3 | Object key outside `avatars/` — the driver always writes `avatars/user_...`; check the bucket name |
