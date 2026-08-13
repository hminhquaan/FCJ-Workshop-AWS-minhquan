---
title : "CloudWatch Logs và Log Insights"
date : 2024-01-01
weight : 5
chapter : false
pre : " <b> 5.5.5 </b> "
---

Lambda ghi log vào **CloudWatch Logs**. Bạn có thể xem chúng và chạy **Logs Insights** queries để xử lý sự cố.

#### 5.5.5.1 Xem Lambda logs

1. Mở **CloudWatch console** → **Log groups**.
2. Tìm log group `/aws/lambda/edms-lambda-stack-EdmsApiFunction...`.
3. Mở log stream mới nhất để xem output của function.

![Figure 48. Log groups](/images/5-Workshop/5.5-Edms-operations/log-groups.png)

#### 5.5.5.2 Dùng Logs Insights

1. Mở **CloudWatch** → **Logs Insights**.
2. Chọn Lambda log group.
3. Chạy một query để tìm lỗi:

```sql
fields @timestamp, @message
| filter @message like /ERROR|Exception/
| sort @timestamp desc
| limit 20
```

> **Ghi chú:** Truy vấn log giúp bạn hiểu vì sao một request thất bại, ví dụ token Cognito không hợp lệ hoặc lỗi kết nối database.
