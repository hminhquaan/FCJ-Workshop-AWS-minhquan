---
title : "Cost Monitoring & Alerts"
date : 2024-01-01
weight : 4
chapter : false
pre : " <b> 5.5.4 </b> "
---

Because this workshop builds several AWS services, it is important to monitor and control cost.

#### 5.5.4.1 Create a Billing Alarm

1. Open **Billing** → **Budgets** → **Create budget**.
2. Choose **Cost budget**, set an amount (e.g. $10/month).
3. Set an **alert threshold** at 80% and an email to notify.
4. Click **Create budget**.

#### 5.5.4.2 Use Cost Explorer

1. Open **Billing** → **Cost Explorer**.
2. Group by **Service** to see which service (e.g. RDS/Aurora, Lambda, API Gateway) costs the most.

> **Tip:** In this architecture, **Aurora** is the main cost driver. Stop or delete it when not in use.

#### 5.5.4.3 Create a CloudWatch alarm (optional)

Create a CloudWatch alarm on an expensive metric, for example a **5XX error** threshold:

1. Open **CloudWatch** → **Alarms** → **Create alarm**.
2. Select the metric `AWS/ApiGateway` → `5XXError` → your API.
3. Set the threshold (e.g. > 0 for 1 datapoint) and the action (SNS topic).
4. Click **Create alarm**.
