---
title : "Kích hoạt Pipeline"
date : 2024-01-01
weight : 10
chapter : false
pre : " <b> 5.4.10 </b> "
---

Pipeline chạy tự động mỗi lần push lên `main`. Bạn cũng có thể kích hoạt thủ công.

#### 5.4.10.1 Kích hoạt bằng push code

```bash
git add .
git commit -m "chore: trigger deployment"
git push origin main
```

Push lên `main` sẽ tự động khởi động workflow `EDMS CI/CD`.

![Figure 28. Run được kích hoạt](/images/5-Workshop/5.4-Edms-deployment/triggered.png)

#### 5.4.10.2 Kích hoạt thủ công (workflow_dispatch)

Nếu workflow của bạn có `workflow_dispatch`, bạn có thể kích hoạt thủ công:

1. Mở tab **Actions**.
2. Chọn workflow `EDMS CI/CD`.
3. Bấm **Run workflow**, chọn nhánh, rồi bấm **Run workflow**.

![Figure 29. Kích hoạt thủ công](/images/5-Workshop/5.4-Edms-deployment/manual-trigger.png)

> **Ghi chú:** Chỉ các job thỏa điều kiện `if` của job deploy mới thực sự deploy. Job deploy chỉ chạy trên nhánh `main`.
