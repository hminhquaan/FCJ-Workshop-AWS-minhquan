---
title: "Week 6 Worklog"
date: 2024-01-01
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
---

### Week 6 Objectives:

* Learn AWS Step Functions and the Wait for Task Token pattern.
* Build the document approval workflow.
* Add SNS email notifications.

### Tasks to be carried out this week:

| No. | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| 1 | - Study AWS Step Functions and state machine concepts | 27/07/2026 | 28/07/2026 | |
| 2 | - Create the SNS topic and email subscription | 28/07/2026 | 28/07/2026 | |
| 3 | - Design the approval state machine (CaptureToken â†’ Decision â†’ Mark â†’ Notify) | 29/07/2026 | 29/07/2026 | |
| 4 | - Integrate Step Functions into the backend (startExecution, SendTaskSuccess) | 30/07/2026 | 31/07/2026 | |
| 5 | - Handle the task token callback and update document status | 31/07/2026 | 01/08/2026 | |
| 6 | - Test the full approval flow end-to-end | 01/08/2026 | 02/08/2026 | |

### Week 6 Achievements:

* Understood AWS Step Functions and the **Wait for Task Token** pattern for human approval.
* Created an SNS topic and confirmed an email subscription.
* Designed and defined the approval state machine (ASL).
* Integrated Step Functions into the backend: `startExecution` on submit, `SendTaskSuccess` on approve/reject.
* Stored the task token and handled the callback to update document status.
* Tested the full approval flow end-to-end and verified the SNS email notification.
