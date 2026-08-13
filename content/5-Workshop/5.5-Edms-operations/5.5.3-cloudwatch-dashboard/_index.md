---
title : "CloudWatch Dashboard"
date : 2024-01-01
weight : 3
chapter : false
pre : " <b> 5.5.3 </b> "
---

A **CloudWatch Dashboard** gives you a single view of your application's health and performance.

#### 5.5.3.1 Create a dashboard

1. Open the **CloudWatch console** → **Dashboards** → **Create dashboard**.
2. Give the dashboard a name, e.g. `edms-dashboard`, and click **Create**.

![Figure 43. Create dashboard](/images/5-Workshop/5.5-Edms-operations/create-dashboard.png)

#### 5.5.3.2 Add widgets

1. Click **Add widget** and choose **Line**.
2. Select a metric, e.g.:
+ **AWS/Lambda** → `edms-lambda-stack-EdmsApiFunction` → **Invocations** and **Errors**
+ **AWS/API Gateway** → your API → **Count** and **4XXError** / **5XXError**
+ **AWS/Step Functions** → your state machine → **ExecutionsSucceeded**
3. Configure the period (e.g. 5 minutes) and click **Create widget**.

![Figure 44. Add widget](/images/5-Workshop/5.5-Edms-operations/add-widget.png)

4. Add several widgets and arrange them, then click **Save dashboard**.

> **Note:** A dashboard is read-only and does not cost much; it helps you spot issues at a glance.
