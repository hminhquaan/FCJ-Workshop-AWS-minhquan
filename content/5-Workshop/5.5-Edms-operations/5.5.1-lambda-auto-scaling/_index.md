---
title : "Lambda Concurrency & Auto-Scaling"
date : 2024-01-01
weight : 1
chapter : false
pre : " <b> 5.5.1 </b> "
---

Lambda scales automatically to handle incoming requests. You can control this scaling with **reserved concurrency** and monitor it with **provisioned concurrency** if needed.

#### 5.5.1.1 How Lambda scaling works

By default, Lambda runs multiple instances of your function in parallel to handle concurrent requests. There is **no server to manage** — AWS scales it automatically based on traffic.

#### 5.5.1.2 Configure reserved concurrency (optional)

Reserved concurrency caps how many concurrent executions your function can use, protecting downstream resources like the database:

1. Open the **Lambda console** → your function.
2. Open the **Configuration** tab → **Concurrency**.
3. Click **Edit** and set **Reserved concurrency** (e.g. 5).
4. Click **Save**.

![Figure 38. Lambda concurrency](/images/5-Workshop/5.5-Edms-operations/lambda-concurrency.png)

> **Note:** In `template.yaml` this can be set with `ReservedConcurrentExecutions` on the function resource.

#### 5.5.1.3 Monitor scaling

In the **Monitor** tab you can view the **Invocations** and **Concurrent executions** graphs to confirm scaling under load.

![Figure 39. Lambda monitoring](/images/5-Workshop/5.5-Edms-operations/lambda-monitor.png)
