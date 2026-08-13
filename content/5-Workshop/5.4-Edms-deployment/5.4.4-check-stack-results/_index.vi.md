---
title : "Xác minh Kết quả Stack và Lấy Outputs"
date : 2024-01-01
weight : 4
chapter : false
pre : " <b> 5.4.4 </b> "
---

Sau khi stack được tạo, hãy xác minh các tài nguyên tồn tại và lấy deploy role ARN.

#### 5.4.4.1 Xác minh trạng thái stack

1. Mở **CloudFormation console** → **Stacks**.
2. Xác nhận `edms-iam-stack` có trạng thái **CREATE_COMPLETE**.

![Figure 17. Stack hoàn tất](/images/5-Workshop/5.4-Edms-deployment/stack-complete.png)

3. Trong tab **Resources**, xác nhận role `github-actions-deploy-role` và OIDC provider tồn tại.

#### 5.4.4.2 Lấy deploy role ARN

1. Mở tab **Outputs** của stack.
2. Sao chép giá trị `DeployRoleArn`, ví dụ `arn:aws:iam::<account-id>:role/github-actions-deploy-role`.

![Figure 18. Stack outputs](/images/5-Workshop/5.4-Edms-deployment/stack-outputs.png)

3. Bạn sẽ lưu ARN này thành secret `AWS_DEPLOY_ROLE_ARN` trên GitHub (phần tiếp theo).

> **Ghi chú:** Ngoài ra, OIDC role có thể được tạo thủ công trong IAM (xem 5.3.3). CloudFormation được ưa dùng vì versioned và tái tạo được.
