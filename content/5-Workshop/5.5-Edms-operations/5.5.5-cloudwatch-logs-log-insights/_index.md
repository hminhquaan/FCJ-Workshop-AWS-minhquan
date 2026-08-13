---
title : "CloudWatch Logs and Log Insights"
date : 2024-01-01
weight : 5
chapter : false
pre : " <b> 5.5.5 </b> "
---

Lambda writes logs to **CloudWatch Logs**. You can inspect them and run **Logs Insights** queries to troubleshoot.

#### 5.5.5.1 View Lambda logs

1. Open the **CloudWatch console** → **Log groups**.
2. Find the log group `/aws/lambda/edms-lambda-stack-EdmsApiFunction...`.
3. Open the latest log stream to see the function output.

![Figure 48. Log groups](/images/5-Workshop/5.5-Edms-operations/log-groups.png)

#### 5.5.5.2 Use Logs Insights

1. Open **CloudWatch** → **Logs Insights**.
2. Select the Lambda log group.
3. Run a query to find errors:

```sql
fields @timestamp, @message
| filter @message like /ERROR|Exception/
| sort @timestamp desc
| limit 20
```

![Figure 49. Logs Insights](/images/5-Workshop/5.5-Edms-operations/logs-insights.png)

> **Note:** Querying logs helps you understand why a request failed, e.g. an invalid Cognito token or a database connection error.
