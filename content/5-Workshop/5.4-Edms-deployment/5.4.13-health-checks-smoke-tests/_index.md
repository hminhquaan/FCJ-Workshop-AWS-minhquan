---
title : "Health Checks and Smoke Tests Post-Deploy"
date : 2024-01-01
weight : 13
chapter : false
pre : " <b> 5.4.13 </b> "
---

After deployment, run health checks and smoke tests to verify the API works end-to-end.

#### 5.4.13.1 Test the login endpoint

```bash
curl -X POST https://<invoke-url>/auth/login \
  -H "Content-Type: application/json" \
  -d '{"email":"admin@edms.vn","password":"your-password"}'
```

A successful response returns a token and the user's role.

![Figure 35. Login test](/images/5-Workshop/5.4-Edms-deployment/login-test.png)

#### 5.4.13.2 Smoke test the approval workflow

1. Using a **USER** account, call `POST /approval/submit` with a `documentId`.
2. Confirm the document becomes `PENDING`.
3. Using a **MANAGER** account, call `POST /approval/approve` with the same `documentId`.
4. Confirm the document becomes `APPROVED` and you receive the **SNS email**.

![Figure 36. Approval smoke test](/images/5-Workshop/5.4-Edms-deployment/approval-test.png)

#### 5.4.13.3 Check Step Functions execution

1. Open the **Step Functions console** → your state machine → **Executions**.
2. You should see a **SUCCEEDED** execution for the document you approved.

![Figure 37. Execution succeeded](/images/5-Workshop/5.4-Edms-deployment/execution-succeeded.png)

> **Note:** The full ASL definition is in the repository (`backend/template.yaml`, resource `DocumentApprovalStateMachine`). It uses `waitForTaskToken` so the workflow can wait for a manager's decision.
