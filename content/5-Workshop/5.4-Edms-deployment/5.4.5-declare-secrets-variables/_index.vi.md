---
title : "Khai báo Secrets và Variables trên GitHub"
date : 2024-01-01
weight : 5
chapter : false
pre : " <b> 5.4.5 </b> "
---

CI/CD pipeline cần các giá trị cấu hình (ARN, ID, credentials). Lưu chúng thành **GitHub Secrets** để không commit vào repository.

#### 5.4.5.1 Các secret cần thiết

Thêm các secret sau trong **GitHub** → **Settings** → **Secrets and variables** → **Actions**:

| Secret | Giá trị |
|--------|---------|
| `AWS_DEPLOY_ROLE_ARN` | Deploy role ARN từ 5.4.4 |
| `COGNITO_USER_POOL_ID` | Cognito User Pool ID từ 5.3.4 |
| `COGNITO_CLIENT_ID` | Cognito Client ID từ 5.3.4 |
| `AURORA_ENDPOINT` | Aurora endpoint từ 5.3.2 |
| `DB_USER_AWS` | Aurora master username |
| `DB_PASS_AWS` | Aurora master password |
| `AWS_S3_BUCKET` | S3 bucket name từ 5.3.1 |
| `SNS_TOPIC_ARN` | SNS topic ARN (tạo sau trong 5.4.8) |
| `BACKEND_LAMBDA_ARN` | Lambda function ARN |

![Figure 19. GitHub secrets](/images/5-Workshop/5.4-Edms-deployment/secrets.png)

#### 5.4.5.2 Thêm một secret

1. Mở repository của bạn trên GitHub → **Settings** → **Secrets and variables** → **Actions**.
2. Bấm **New repository secret**.
3. Nhập **Name** và **Value**, rồi bấm **Add secret**.

> **Thực hành tốt:** Không bao giờ commit secret hoặc file `.env`. Tất cả secret được inject lúc deploy bởi workflow.
