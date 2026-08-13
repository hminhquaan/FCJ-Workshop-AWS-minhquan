---
title : "Clean Up Resources"
date : 2024-01-01
weight : 7
chapter : false
pre : " <b> 5.5.7 </b> "
---

To avoid ongoing costs, delete the resources you created in this workshop.

#### 5.5.7.1 Delete the SAM/CloudFormation stack

The Lambda, API Gateway, Step Functions, and IAM resources created by SAM can be removed with one command (or by deleting the stack in the console):

```bash
sam delete --stack-name edms-lambda-stack --no-prompts
```

![Figure 53. Delete stack](/images/5-Workshop/5.5-Edms-operations/delete-stack.png)

#### 5.5.7.2 Delete remaining resources manually

Delete the following in the console:

1. **Amplify** — delete the app.
2. **Step Functions** — delete the state machine.
3. **SNS** — delete the `edms-notifications` topic.
4. **Cognito** — delete the User Pool.
5. **S3** — empty and delete the buckets (including object versions).
6. **Aurora** — **delete the cluster** (the main source of cost).
7. **IAM** — delete the deploy role (after removing any CloudFormation stack that references it).

![Figure 54. Delete resources](/images/5-Workshop/5.5-Edms-operations/delete-resources.png)

#### 5.5.7.2 Verify zero cost

Use **Cost Explorer** to confirm no service is still incurring charges. Optionally set a **$0 budget** alert to be notified if anything remains.

> **Best practice:** After cleanup, confirm the stack list and resource list are empty so you do not leave a running bill.
