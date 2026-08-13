---
title : "Create Step Functions State Machine"
date : 2024-01-01
weight : 8
chapter : false
pre : " <b> 5.4.8 </b> "
---

EDMS orchestrates the **document approval workflow** with **AWS Step Functions**. This section creates the SNS topic and the state machine.

#### 5.4.8.1 Create the SNS topic

1. Open the **SNS console** → **Topics** → **Create topic**.
2. **Type:** Standard. **Name:** `edms-notifications`.
3. Click **Create topic**.
4. In the topic, create a **subscription** with **Protocol = Email** and your email address; confirm it from the email.

![Figure 23. SNS topic](/images/5-Workshop/5.4-Edms-deployment/sns-topic.png)

> Note down the **topic ARN** — you will set it as the `SNS_TOPIC_ARN` secret.

#### 5.4.8.2 Create the state machine

1. Open the **Step Functions console** → **State machines** → **Create state machine**.
2. Choose **Author with code**, type **Standard**.
3. Paste the **ASL** definition (see section 5.4.13 note, or the repository's `template.yaml`). The flow is:

```
CaptureToken (waitForTaskToken)
   → Decision (Choice)
   → MarkApproved / MarkRejected   (update DB via Lambda)
   → NotifyApproved / NotifyRejected  (SNS publish)
```

![Figure 24. State machine](/images/5-Workshop/5.4-Edms-deployment/statemachine.png)

4. **Permissions:** create a new role allowing `lambda:InvokeFunction` and `sns:Publish`.
5. **Name:** `DocumentApprovalStateMachine`. Click **Create state machine**.

#### 5.4.8.3 Note the ARN

Note down the **state machine ARN** — the Lambda needs it (`STEP_FUNCTIONS_ARN`) to start executions.

> **Key pattern:** The `CaptureToken` state uses `.waitForTaskToken`. It can wait indefinitely for a manager's decision, which a Lambda alone cannot do.
