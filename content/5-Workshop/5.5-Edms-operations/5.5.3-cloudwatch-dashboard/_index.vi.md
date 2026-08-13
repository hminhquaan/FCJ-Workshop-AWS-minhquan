---
title : "CloudWatch Dashboard"
date : 2024-01-01
weight : 3
chapter : false
pre : " <b> 5.5.3 </b> "
---

Một **CloudWatch Dashboard** cung cấp một cái nhìn tổng hợp về sức khỏe và hiệu năng của ứng dụng.

#### 5.5.3.1 Tạo dashboard

1. Mở **CloudWatch console** → **Dashboards** → **Create dashboard**.
2. Đặt tên dashboard, ví dụ `edms-dashboard`, và bấm **Create**.

![Figure 43. Tạo dashboard](/images/5-Workshop/5.5-Edms-operations/create-dashboard.png)

#### 5.5.3.2 Thêm widgets

1. Bấm **Add widget** và chọn **Line**.
2. Chọn một metric, ví dụ:
+ **AWS/Lambda** → `edms-lambda-stack-EdmsApiFunction` → **Invocations** và **Errors**
+ **AWS/API Gateway** → API của bạn → **Count** và **4XXError** / **5XXError**
+ **AWS/Step Functions** → state machine của bạn → **ExecutionsSucceeded**
3. Cấu hình period (ví dụ 5 phút) và bấm **Create widget**.

![Figure 44. Thêm widget](/images/5-Workshop/5.5-Edms-operations/add-widget.png)

4. Thêm nhiều widgets và sắp xếp, rồi bấm **Save dashboard**.

> **Ghi chú:** Dashboard là read-only và chi phí thấp; nó giúp bạn phát hiện vấn đề chỉ trong một cái nhìn.
