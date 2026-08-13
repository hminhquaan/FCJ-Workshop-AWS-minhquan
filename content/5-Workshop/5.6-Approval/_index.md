---
title : "Approval Workflow (Step Functions & SNS)"
date : 2024-01-01
weight : 6
chapter : false
pre : " <b> 5.6. </b> "
---

In this section you will build the **document approval workflow** using **AWS Step Functions** and **Amazon SNS**.

#### The problem

Document approval is a **long-running, human-in-the-loop** process: after a user submits a document, a manager must approve or reject it — possibly hours or days later. A Lambda function cannot wait that long (its maximum timeout is 15 minutes). **AWS Step Functions** solves this with the **Wait for Task Token** pattern, which can wait indefinitely for a human decision.

#### Create the SNS topic

1. Open the **SNS console** → **Topics** → **Create topic**.
2. **Type:** Standard. **Name:** `edms-notifications`.
3. Choose **Create topic**.
4. In the topic, choose **Create subscription**:
+ **Protocol:** Email.
+ **Endpoint:** your email address.
5. Confirm the subscription from the email you receive (you must click the confirmation link).

![SNS topic](/images/5-Workshop/5.6-Approval/sns-topic.png)

> **Note:** Note down the **topic ARN** — the backend needs it.

#### Create the Step Functions state machine

1. Open the **Step Functions console** → **State machines** → **Create state machine**.
2. Choose **Author with code**, type **Standard**.
3. Paste the following **ASL** (Amazon States Language) definition:

```json
{
  "Comment": "EDMS Document Approval Workflow",
  "StartAt": "CaptureToken",
  "States": {
    "CaptureToken": {
      "Type": "Task",
      "Resource": "arn:aws:states:::lambda:invoke.waitForTaskToken",
      "Parameters": {
        "FunctionName": "<LAMBDA_ARN>",
        "Payload": {
          "edmsInternal": "captureToken",
          "documentId.$": "$.documentId",
          "taskToken.$": "$$.Task.Token"
        }
      },
      "Next": "Decision"
    },
    "Decision": {
      "Type": "Choice",
      "Choices": [
        { "Variable": "$.decision", "StringEquals": "APPROVED", "Next": "MarkApproved" },
        { "Variable": "$.decision", "StringEquals": "REJECTED", "Next": "MarkRejected" }
      ],
      "Default": "MarkRejected"
    },
    "MarkApproved": {
      "Type": "Task",
      "Resource": "arn:aws:states:::lambda:invoke",
      "Parameters": {
        "FunctionName": "<LAMBDA_ARN>",
        "Payload": {
          "edmsInternal": "markStatus",
          "documentId.$": "$.documentId",
          "decision.$": "$.decision",
          "actedBy.$": "$.actedBy"
        }
      },
      "Next": "NotifyApproved"
    },
    "MarkRejected": {
      "Type": "Task",
      "Resource": "arn:aws:states:::lambda:invoke",
      "Parameters": {
        "FunctionName": "<LAMBDA_ARN>",
        "Payload": {
          "edmsInternal": "markStatus",
          "documentId.$": "$.documentId",
          "decision.$": "$.decision",
          "actedBy.$": "$.actedBy",
          "reason.$": "$.reason"
        }
      },
      "Next": "NotifyRejected"
    },
    "NotifyApproved": {
      "Type": "Task",
      "Resource": "arn:aws:states:::sns:publish",
      "Parameters": {
        "TopicArn": "<SNS_TOPIC_ARN>",
        "Subject": "EDMS - Document Approved",
        "Message": "Your document has been APPROVED."
      },
      "End": true
    },
    "NotifyRejected": {
      "Type": "Task",
      "Resource": "arn:aws:states:::sns:publish",
      "Parameters": {
        "TopicArn": "<SNS_TOPIC_ARN>",
        "Subject": "EDMS - Document Rejected",
        "Message": "Your document has been REJECTED."
      },
      "End": true
    }
  }
}
```

![State machine definition](/images/5-Workshop/5.6-Approval/statemachine-def.png)

4. **Permissions:** create a new role. Make sure the role allows:
+ `lambda:InvokeFunction`
+ `sns:Publish`
5. **Name:** `DocumentApprovalStateMachine`. Choose **Create state machine**.

![State machine created](/images/5-Workshop/5.6-Approval/statemachine-created.png)

> **Note:** Note down the **state machine ARN** — the backend needs it to start executions.

#### How the workflow works

```
USER submit ──▶ Lambda: startExecution({documentId})
                     │
                     ▼
  [CaptureToken: Task .waitForTaskToken] ── Lambda lưu task token vào DB, treo chờ
                     │           chờ SendTaskSuccess / SendTaskFailure
                     ▼
        [Decision: Choice]
        APPROVED │            │ REJECTED
                ▼            ▼
        [MarkApproved]  [MarkRejected]   (Lambda cập nhật DB status + history)
                │            │
                ▼            ▼
        [NotifyApproved] [NotifyRejected] (Step Functions → SNS: publish email)
                └───── End ─────┘
```

1. **Submit** — the API `/approval/submit` sets the document to `PENDING` and calls `startExecution`.
2. **CaptureToken** — the `CaptureToken` state (`.waitForTaskToken`) invokes Lambda, which stores the **task token** in the database, then **waits** for the manager's decision (without any time limit).
3. **Approve / Reject** — the API calls `SendTaskSuccess(token, {decision, actedBy, reason})`, which "wakes up" the state machine.
4. **Decision** — a Choice state branches by the decision.
5. **MarkStatus** — Step Functions invokes Lambda to update the database (`status = APPROVED / REJECTED`) and write the approval history.
6. **Notify** — Step Functions publishes to **SNS**, which sends the email notification.

#### Test the workflow

1. Start an execution with the input:
```json
{ "documentId": "doc-1" }
```
2. The execution reaches the `CaptureToken` state and **waits**.
3. From the application (or by calling `SendTaskSuccess` with the task token), approve the document.
4. Watch the state machine complete: `CaptureToken → Decision → MarkApproved → NotifyApproved`.

![Execution](/images/5-Workshop/5.6-Approval/execution.png)

You should also receive the **SNS email notification**.

> **Key takeaway:** Step Functions gives the approval process a **clear, auditable execution history**, lets it **wait for a human decision indefinitely**, and each step supports **retry and error handling** — none of which is possible with a single Lambda timeout.
