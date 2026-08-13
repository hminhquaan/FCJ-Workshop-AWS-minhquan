---
title : "Tạo API Gateway"
date : 2024-01-01
weight : 11
chapter : false
pre : " <b> 5.4.11 </b> "
---

**API Gateway** phơi backend Lambda thành một REST API. Trong production điều này được định nghĩa trong `template.yaml` và được SAM tạo tự động.

#### 5.4.11.1 Tạo REST API

1. Mở **API Gateway console** → **Create API**.
2. Chọn **REST API** (không phải HTTP API) và bấm **Build**.

![Figure 30. Tạo API](/images/5-Workshop/5.4-Edms-deployment/create-api.png)

3. **Choose the protocol:** REST. **Create new API.** Đặt tên `edms-api`.
4. Bấm **Create API**.

#### 5.4.11.2 Thêm resource proxy catch-all

1. Trên trang **Resources**, tạo một resource với path `{proxy+}`.
2. Với resource `{proxy+}`, tạo method **ANY**.
3. Đặt **Integration type** là **Lambda Function**, chọn Lambda EDMS và region.

![Figure 31. Proxy method](/images/5-Workshop/5.4-Edms-deployment/proxy-method.png)

4. Lặp lại cho resource gốc `/` với method **ANY** (hoặc `GET` cho health check).

> **Ghi chú:** Resource `{proxy+}` cho phép API Gateway chuyển tiếp mọi path (`/auth/login`, `/documents`, ...) đến Lambda, nơi nó route bên trong ứng dụng Spring Boot.
