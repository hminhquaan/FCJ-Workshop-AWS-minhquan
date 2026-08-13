---
title : "Verify Stack Results and Retrieve Outputs"
date : 2024-01-01
weight : 4
chapter : false
pre : " <b> 5.4.4 </b> "
---

After the stack is created, verify that the resources exist and retrieve the deploy role ARN.

#### 5.4.4.1 Verify the stack status

1. Open the **CloudFormation console** → **Stacks**.
2. Confirm `edms-iam-stack` has status **CREATE_COMPLETE**.

![Figure 17. Stack complete](/images/5-Workshop/5.4-Edms-deployment/stack-complete.png)

3. In the **Resources** tab, confirm the role `github-actions-deploy-role` and the OIDC provider exist.

#### 5.4.4.2 Retrieve the deploy role ARN

1. Open the **Outputs** tab of the stack.
2. Copy the `DeployRoleArn` value, e.g. `arn:aws:iam::<account-id>:role/github-actions-deploy-role`.

![Figure 18. Stack outputs](/images/5-Workshop/5.4-Edms-deployment/stack-outputs.png)

3. You will store this ARN as the `AWS_DEPLOY_ROLE_ARN` secret on GitHub (next section).

> **Note:** Alternatively, the OIDC role can be created manually in IAM (see 5.3.3). CloudFormation is preferred because it is versioned and reproducible.
