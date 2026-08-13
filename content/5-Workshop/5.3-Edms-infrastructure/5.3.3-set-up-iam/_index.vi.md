---
title : "Khởi tạo và Cấu hình IAM"
date : 2024-01-01
weight : 3
chapter : false
pre : " <b> 5.3.3 </b> "
---

IAM cung cấp các định danh và quyền mà EDMS cần: một **role OIDC** cho GitHub Actions (deploy) và **role thực thi Lambda**.

#### 5.3.3.1 Tạo OIDC provider

Để GitHub Actions assume một role (thay vì lưu AWS key dài hạn), hãy đăng ký OIDC provider của GitHub:

1. Mở **IAM console** → **Identity providers** → **Add provider**.
2. **Provider type:** **OpenID Connect**.
3. **Provider URL:** `https://token.actions.githubusercontent.com`.
4. **Audience:** `sts.amazonaws.com`.
5. Bấm **Add provider**.

![Figure 8. Thêm OIDC provider](/images/5-Workshop/5.3-Edms-infrastructure/oidc-provider.png)

#### 5.3.3.2 Tạo deploy role

1. Mở **IAM** → **Roles** → **Create role**.
2. **Trusted entity type:** **Web identity**.
3. Chọn OIDC provider GitHub và audience `sts.amazonaws.com`.
4. Thêm trust policy cho phép repository assume role (xem 5.4 để biết policy chính xác).
5. Trong **Permissions**, đính kèm policy deploy rộng (CloudFormation, Lambda, S3, API Gateway, IAM, v.v.).
6. Đặt tên role `github-actions-deploy-role` và bấm **Create role**.

![Figure 9. Deploy role](/images/5-Workshop/5.3-Edms-infrastructure/deploy-role.png)

#### 5.3.3.3 Tạo role thực thi Lambda

1. Mở **IAM** → **Roles** → **Create role**.
2. **Trusted entity type:** **AWS service** → **Lambda**.
3. Đính kèm policy cho phép tối thiểu:
+ `s3:PutObject`, `s3:GetObject`, `s3:DeleteObject`, `s3:ListBucket`
+ `cognito-idp:InitiateAuth`, `cognito-idp:GetUser`
+ `sns:Publish`
+ `states:StartExecution`, `states:SendTaskSuccess`, `states:SendTaskFailure`
+ `logs:CreateLogGroup`, `logs:CreateLogStream`, `logs:PutLogEvents`
4. Đặt tên role `edms-lambda-role` và bấm **Create role**.

![Figure 10. Lambda role](/images/5-Workshop/5.3-Edms-infrastructure/lambda-role.png)

> **Thực hành tốt:** Chỉ cấp những quyền cần thiết cho mỗi role (least-privilege). Không bao giờ đặt AWS key trong code.
