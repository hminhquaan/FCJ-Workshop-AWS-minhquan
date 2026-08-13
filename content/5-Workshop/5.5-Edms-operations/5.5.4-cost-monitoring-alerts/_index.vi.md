---
title : "Giám sát Chi phí & Cảnh báo"
date : 2024-01-01
weight : 4
chapter : false
pre : " <b> 5.5.4 </b> "
---

Vì workshop này xây dựng nhiều dịch vụ AWS, việc giám sát và kiểm soát chi phí là rất quan trọng.

#### 5.5.4.1 Tạo Billing Alarm

1. Mở **Billing** → **Budgets** → **Create budget**.
2. Chọn **Cost budget**, đặt một mức tiền (ví dụ $10/tháng).
3. Đặt **alert threshold** ở 80% và một email để thông báo.
4. Bấm **Create budget**.

![Figure 45. Budget](/images/5-Workshop/5.5-Edms-operations/budget.png)

#### 5.5.4.2 Dùng Cost Explorer

1. Mở **Billing** → **Cost Explorer**.
2. Group by **Service** để xem dịch vụ nào (ví dụ RDS/Aurora, Lambda, API Gateway) tốn nhiều nhất.

![Figure 46. Cost Explorer](/images/5-Workshop/5.5-Edms-operations/cost-explorer.png)

> **Mẹo:** Trong kiến trúc này, **Aurora** là nguồn chi phí chính. Hãy stop hoặc xóa nó khi không dùng.

#### 5.5.4.3 Tạo CloudWatch alarm (tùy chọn)

Tạo CloudWatch alarm trên một metric, ví dụ ngưỡng **5XX error**:

1. Mở **CloudWatch** → **Alarms** → **Create alarm**.
2. Chọn metric `AWS/ApiGateway` → `5XXError` → API của bạn.
3. Đặt ngưỡng (ví dụ > 0 cho 1 datapoint) và action (SNS topic).
4. Bấm **Create alarm**.

![Figure 47. Alarm](/images/5-Workshop/5.5-Edms-operations/alarm.png)
