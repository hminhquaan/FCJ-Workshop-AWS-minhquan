---
title : "Initialize and Configure IAM"
date : 2024-01-01
weight : 3
chapter : false
pre : " <b> 5.3.3 </b> "
---

IAM provides the identities and permissions that EDMS needs: an **OIDC role** for GitHub Actions (deploy) and the **Lambda execution role**.

#### 5.3.3.1 Create the OIDC provider

To let GitHub Actions assume a role (instead of storing long-term AWS keys), register GitHub's OIDC provider:

1. Open the **IAM console** → **Identity providers** → **Add provider**.
2. **Provider type:** **OpenID Connect**.
3. **Provider URL:** `https://token.actions.githubusercontent.com`.
4. **Audience:** `sts.amazonaws.com`.
5. Click **Add provider**.

![Figure 8. Add OIDC provider](/images/5-Workshop/5.3-Edms-infrastructure/oidc-provider.png)

#### 5.3.3.2 Create the deploy role

1. Open **IAM** → **Roles** → **Create role**.
2. **Trusted entity type:** **Web identity**.
3. Select the GitHub OIDC provider and audience `sts.amazonaws.com`.
4. Add a trust policy that allows the repository to assume the role (see 5.4 for the exact policy).
5. Under **Permissions**, attach a broad deploy policy (CloudFormation, Lambda, S3, API Gateway, IAM, etc.).
6. Name the role `github-actions-deploy-role` and click **Create role**.

![Figure 9. Deploy role](/images/5-Workshop/5.3-Edms-infrastructure/deploy-role.png)

#### 5.3.3.3 Create the Lambda execution role

1. Open **IAM** → **Roles** → **Create role**.
2. **Trusted entity type:** **AWS service** → **Lambda**.
3. Attach a policy that allows at least:
+ `s3:PutObject`, `s3:GetObject`, `s3:DeleteObject`, `s3:ListBucket`
+ `cognito-idp:InitiateAuth`, `cognito-idp:GetUser`
+ `sns:Publish`
+ `states:StartExecution`, `states:SendTaskSuccess`, `states:SendTaskFailure`
+ `logs:CreateLogGroup`, `logs:CreateLogStream`, `logs:PutLogEvents`
4. Name the role `edms-lambda-role` and click **Create role**.

![Figure 10. Lambda role](/images/5-Workshop/5.3-Edms-infrastructure/lambda-role.png)

> **Best practice:** Grant only the permissions each role needs (least-privilege). Never put AWS keys in code.
