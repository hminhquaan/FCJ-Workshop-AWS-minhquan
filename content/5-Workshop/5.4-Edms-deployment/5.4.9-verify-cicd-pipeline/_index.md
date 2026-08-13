---
title : "Verify CI/CD Pipeline"
date : 2024-01-01
weight : 9
chapter : false
pre : " <b> 5.4.9 </b> "
---

After pushing the code and configuring the workflow, verify that the pipeline runs and deploys successfully.

#### 5.4.9.1 Open the workflow runs

1. In your repository, open the **Actions** tab.
2. You should see the `EDMS CI/CD` workflow runs.

![Figure 25. Workflow runs](/images/5-Workshop/5.4-Edms-deployment/workflow-runs.png)

#### 5.4.9.2 Verify each job

Click on the latest run and confirm the three jobs pass:

+ `test-backend` — ✅
+ `build-frontend` — ✅
+ `deploy` — ✅

![Figure 26. Jobs pass](/images/5-Workshop/5.4-Edms-deployment/jobs-pass.png)

#### 5.4.9.3 Verify the stack update

1. Open the **CloudFormation console** → **Stacks**.
2. Confirm the `edms-lambda-stack` status is **UPDATE_COMPLETE**.

![Figure 27. Stack updated](/images/5-Workshop/5.4-Edms-deployment/stack-updated.png)

> **Note:** If a job fails, click it to view the logs and fix the issue, then push again to retrigger the pipeline.
