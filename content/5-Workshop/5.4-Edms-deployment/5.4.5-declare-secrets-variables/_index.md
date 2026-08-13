---
title : "Declare Secrets and Variables on GitHub"
date : 2024-01-01
weight : 5
chapter : false
pre : " <b> 5.4.5 </b> "
---

The CI/CD pipeline needs configuration values (ARNs, IDs, credentials). Store them as **GitHub Secrets** so they are not committed to the repository.

#### 5.4.5.1 Required secrets

Add the following secrets in **GitHub** → **Settings** → **Secrets and variables** → **Actions**:

| Secret | Value |
|--------|-------|
| `AWS_DEPLOY_ROLE_ARN` | The deploy role ARN from 5.4.4 |
| `COGNITO_USER_POOL_ID` | The Cognito User Pool ID from 5.3.4 |
| `COGNITO_CLIENT_ID` | The Cognito Client ID from 5.3.4 |
| `AURORA_ENDPOINT` | The Aurora endpoint from 5.3.2 |
| `DB_USER_AWS` | The Aurora master username |
| `DB_PASS_AWS` | The Aurora master password |
| `AWS_S3_BUCKET` | The S3 bucket name from 5.3.1 |
| `SNS_TOPIC_ARN` | The SNS topic ARN (created later in 5.4.8) |
| `BACKEND_LAMBDA_ARN` | The Lambda function ARN |

![Figure 19. GitHub secrets](/images/5-Workshop/5.4-Edms-deployment/secrets.png)

#### 5.4.5.2 Add a secret

1. Open your repository on GitHub → **Settings** → **Secrets and variables** → **Actions**.
2. Click **New repository secret**.
3. Enter the **Name** and **Value**, then click **Add secret**.

> **Best practice:** Never commit secrets or the `.env` file. All secrets are injected at deploy time by the workflow.
