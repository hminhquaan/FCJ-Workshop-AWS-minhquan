---
title : "Trigger Pipeline"
date : 2024-01-01
weight : 10
chapter : false
pre : " <b> 5.4.10 </b> "
---

The pipeline runs automatically on every push to `main`. You can also trigger it manually.

#### 5.4.10.1 Trigger by pushing code

```bash
git add .
git commit -m "chore: trigger deployment"
git push origin main
```

The push to `main` starts the `EDMS CI/CD` workflow automatically.

![Figure 28. Triggered run](/images/5-Workshop/5.4-Edms-deployment/triggered.png)

#### 5.4.10.2 Trigger manually (workflow_dispatch)

If your workflow includes `workflow_dispatch`, you can trigger it manually:

1. Open the **Actions** tab.
2. Select the `EDMS CI/CD` workflow.
3. Click **Run workflow**, select the branch, and click **Run workflow**.

![Figure 29. Manual trigger](/images/5-Workshop/5.4-Edms-deployment/manual-trigger.png)

> **Note:** Only jobs that satisfy the `if` condition on the deploy job will actually deploy. The deploy job runs only on the `main` branch.
