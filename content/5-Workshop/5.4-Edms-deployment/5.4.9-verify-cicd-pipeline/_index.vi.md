---
title : "Xác minh CI/CD Pipeline"
date : 2024-01-01
weight : 9
chapter : false
pre : " <b> 5.4.9 </b> "
---

Sau khi push code và cấu hình workflow, hãy xác minh pipeline chạy và deploy thành công.

#### 5.4.9.1 Mở các workflow runs

1. Trong repository của bạn, mở tab **Actions**.
2. Bạn sẽ thấy các workflow runs của `EDMS CI/CD`.

![Figure 25. Workflow runs](/images/5-Workshop/5.4-Edms-deployment/workflow-runs.png)

#### 5.4.9.2 Xác minh từng job

Bấm vào run gần nhất và xác nhận ba job đều pass:

+ `test-backend` — ✅
+ `build-frontend` — ✅
+ `deploy` — ✅

![Figure 26. Các job pass](/images/5-Workshop/5.4-Edms-deployment/jobs-pass.png)

#### 5.4.9.3 Xác minh stack được cập nhật

1. Mở **CloudFormation console** → **Stacks**.
2. Xác nhận trạng thái của `edms-lambda-stack` là **UPDATE_COMPLETE**.

![Figure 27. Stack được cập nhật](/images/5-Workshop/5.4-Edms-deployment/stack-updated.png)

> **Ghi chú:** Nếu một job fail, bấm vào nó để xem log và sửa lỗi, rồi push lại để chạy lại pipeline.
