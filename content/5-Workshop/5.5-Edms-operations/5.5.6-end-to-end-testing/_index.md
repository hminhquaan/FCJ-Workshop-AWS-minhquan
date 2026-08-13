---
title : "End-to-End Testing"
date : 2024-01-01
weight : 6
chapter : false
pre : " <b> 5.5.6 </b> "
---

Perform an **end-to-end test** through the deployed web application to verify the full flow works.

#### 5.5.6.1 Open the application

1. Open the **Amplify** URL (default domain).
2. You should see the EDMS login page.

![Figure 50. Login page](/images/5-Workshop/5.5-Edms-operations/login.png)

#### 5.5.6.2 Sign in

1. Sign in with a **USER** account created in Cognito.

![Figure 51. Sign in](/images/5-Workshop/5.5-Edms-operations/signin.png)

#### 5.5.6.3 Test the document lifecycle

1. **Create / upload** a document.
2. **Submit** it for approval (status → `PENDING`).
3. Sign out and sign in as a **MANAGER**.
4. **Approve** the document (status → `APPROVED`).
5. Confirm you received the **SNS email notification**.

![Figure 52. Document approved](/images/5-Workshop/5.5-Edms-operations/approved.png)

> **Success criteria:** The document moves through `DRAFT → PENDING → APPROVED`, the Step Functions execution **succeeds**, and an **email** is delivered.
