---
title : "Quy trình phê duyệt (Step Functions & SNS)"
date : 2024-01-01
weight : 6
chapter : false
pre : " <b> 5.6. </b> "
---

Trong phần này bạn sẽ xây dựng **quy trình phê duyệt tài liệu** bằng **AWS Step Functions** và **Amazon SNS**.

#### Vấn đề

Phê duyệt tài liệu là một quy trình **kéo dài, có con người tham gia**: sau khi người dùng nộp tài liệu, một manager phải duyệt hoặc từ chối — có thể sau nhiều giờ hoặc nhiều ngày. Một Lambda function không thể chờ lâu như vậy (timeout tối đa chỉ 15 phút). **AWS Step Functions** giải quyết điều này bằng mẫu **Wait for Task Token**, có thể chờ vô hạn cho một quyết định của con người.

#### Tạo SNS topic

1. Mở **SNS console** → **Topics** → **Create topic**.
2. **Type:** Standard. **Name:** `edms-notifications`.
3. Chọn **Create topic**.
4. Trong topic, chọn **Create subscription**:
+ **Protocol:** Email.
+ **Endpoint:** địa chỉ email của bạn.
5. Xác nhận subscription từ email bạn nhận được (phải bấm link xác nhận).

![SNS topic](/images/5-Workshop/5.6-Approval/sns-topic.png)

> **Ghi chú:** Ghi lại **topic ARN** — backend cần nó.

#### Tạo Step Functions state machine

1. Mở **Step Functions console** → **State machines** → **Create state machine**.
2. Chọn **Author with code**, loại **Standard**.
3. Dán định nghĩa **ASL** (Amazon States Language) sau:

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

4. **Permissions:** tạo role mới. Đảm bảo role cho phép:
+ `lambda:InvokeFunction`
+ `sns:Publish`
5. **Name:** `DocumentApprovalStateMachine`. Chọn **Create state machine**.

![State machine created](/images/5-Workshop/5.6-Approval/statemachine-created.png)

> **Ghi chú:** Ghi lại **state machine ARN** — backend cần nó để khởi động execution.

#### Quy trình hoạt động thế nào

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

1. **Submit** — API `/approval/submit` đặt tài liệu thành `PENDING` và gọi `startExecution`.
2. **CaptureToken** — state `CaptureToken` (`.waitForTaskToken`) invoke Lambda, Lambda lưu **task token** vào database, rồi **chờ** quyết định của manager (không giới hạn thời gian).
3. **Approve / Reject** — API gọi `SendTaskSuccess(token, {decision, actedBy, reason})`, "đánh thức" state machine.
4. **Decision** — một state Choice rẽ nhánh theo quyết định.
5. **MarkStatus** — Step Functions invoke Lambda để cập nhật database (`status = APPROVED / REJECTED`) và ghi lịch sử phê duyệt.
6. **Notify** — Step Functions publish đến **SNS**, gửi email thông báo.

#### Test quy trình

1. Khởi động một execution với input:
```json
{ "documentId": "doc-1" }
```
2. Execution đạt đến state `CaptureToken` và **chờ**.
3. Từ ứng dụng (hoặc gọi `SendTaskSuccess` với task token), duyệt tài liệu.
4. Quan sát state machine hoàn thành: `CaptureToken → Decision → MarkApproved → NotifyApproved`.

![Execution](/images/5-Workshop/5.6-Approval/execution.png)

Bạn cũng nên nhận được **email thông báo SNS**.

> **Điểm mấu chốt:** Step Functions mang lại **lịch sử execution rõ ràng, có thể audit**, cho phép **chờ quyết định con người vô hạn**, và mỗi bước hỗ trợ **retry và xử lý lỗi** — những điều không thể làm với timeout của một Lambda.
