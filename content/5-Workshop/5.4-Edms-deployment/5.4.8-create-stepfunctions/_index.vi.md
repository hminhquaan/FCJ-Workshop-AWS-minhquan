---
title : "Tạo Step Functions State Machine"
date : 2024-01-01
weight : 8
chapter : false
pre : " <b> 5.4.8 </b> "
---

EDMS điều phối **quy trình phê duyệt tài liệu** bằng **AWS Step Functions**. Phần này tạo SNS topic và state machine.

#### 5.4.8.1 Tạo SNS topic

1. Mở **SNS console** → **Topics** → **Create topic**.
2. **Type:** Standard. **Name:** `edms-notifications`.
3. Bấm **Create topic**.
4. Trong topic, tạo một **subscription** với **Protocol = Email** và email của bạn; xác nhận từ email nhận được.

![Figure 23. SNS topic](/images/5-Workshop/5.4-Edms-deployment/sns-topic.png)

> Ghi lại **topic ARN** — bạn sẽ đặt nó làm secret `SNS_TOPIC_ARN`.

#### 5.4.8.2 Tạo state machine

1. Mở **Step Functions console** → **State machines** → **Create state machine**.
2. Chọn **Author with code**, loại **Standard**.
3. Dán định nghĩa **ASL** (xem ghi chú mục 5.4.13, hoặc `template.yaml` trong repository). Flow:

```
CaptureToken (waitForTaskToken)
   → Decision (Choice)
   → MarkApproved / MarkRejected   (cập nhật DB qua Lambda)
   → NotifyApproved / NotifyRejected  (SNS publish)
```

![Figure 24. State machine](/images/5-Workshop/5.4-Edms-deployment/statemachine.png)

4. **Permissions:** tạo role mới cho phép `lambda:InvokeFunction` và `sns:Publish`.
5. **Name:** `DocumentApprovalStateMachine`. Bấm **Create state machine**.

#### 5.4.8.3 Ghi lại ARN

Ghi lại **state machine ARN** — Lambda cần nó (`STEP_FUNCTIONS_ARN`) để khởi động execution.

> **Mẫu mấu chốt:** State `CaptureToken` dùng `.waitForTaskToken`. Nó có thể chờ vô hạn quyết định của manager, điều một Lambda đơn lẻ không thể làm được.
