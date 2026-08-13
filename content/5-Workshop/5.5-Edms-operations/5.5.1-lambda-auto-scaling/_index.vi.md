---
title : "Lambda Concurrency & Auto-Scaling"
date : 2024-01-01
weight : 1
chapter : false
pre : " <b> 5.5.1 </b> "
---

Lambda tự động scale để xử lý các request đến. Bạn có thể kiểm soát việc scale này bằng **reserved concurrency** và giám sát bằng **provisioned concurrency** nếu cần.

#### 5.5.1.1 Cách Lambda scaling hoạt động

Mặc định, Lambda chạy nhiều instance của function song song để xử lý request đồng thời. **Không có server phải quản lý** — AWS tự động scale dựa trên lưu lượng.

#### 5.5.1.2 Cấu hình reserved concurrency (tùy chọn)

Reserved concurrency giới hạn số execution đồng thời function có thể dùng, bảo vệ các tài nguyên downstream như database:

1. Mở **Lambda console** → function của bạn.
2. Mở tab **Configuration** → **Concurrency**.
3. Bấm **Edit** và đặt **Reserved concurrency** (ví dụ 5).
4. Bấm **Save**.

![Figure 38. Lambda concurrency](/images/5-Workshop/5.5-Edms-operations/lambda-concurrency.png)

> **Ghi chú:** Trong `template.yaml` có thể đặt bằng `ReservedConcurrentExecutions` trên resource function.

#### 5.5.1.3 Giám sát scaling

Trong tab **Monitor** bạn có thể xem các biểu đồ **Invocations** và **Concurrent executions** để xác nhận scaling dưới tải.

![Figure 39. Giám sát Lambda](/images/5-Workshop/5.5-Edms-operations/lambda-monitor.png)
