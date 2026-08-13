---
title : "Health Checks và Smoke Tests sau Deploy"
date : 2024-01-01
weight : 13
chapter : false
pre : " <b> 5.4.13 </b> "
---

Sau khi deploy, chạy health checks và smoke tests để xác minh API hoạt động end-to-end.

#### 5.4.13.1 Test endpoint đăng nhập

```bash
curl -X POST https://<invoke-url>/auth/login \
  -H "Content-Type: application/json" \
  -d '{"email":"admin@edms.vn","password":"mat-khau-cua-ban"}'
```

Phản hồi thành công trả về token và vai trò của người dùng.

![Figure 35. Test đăng nhập](/images/5-Workshop/5.4-Edms-deployment/login-test.png)

#### 5.4.13.2 Smoke test quy trình phê duyệt

1. Dùng tài khoản **USER**, gọi `POST /approval/submit` với một `documentId`.
2. Xác nhận tài liệu trở thành `PENDING`.
3. Dùng tài khoản **MANAGER**, gọi `POST /approval/approve` với cùng `documentId`.
4. Xác nhận tài liệu trở thành `APPROVED` và bạn nhận được **email SNS**.

![Figure 36. Smoke test phê duyệt](/images/5-Workshop/5.4-Edms-deployment/approval-test.png)

#### 5.4.13.3 Kiểm tra Step Functions execution

1. Mở **Step Functions console** → state machine của bạn → **Executions**.
2. Bạn sẽ thấy một execution **SUCCEEDED** cho tài liệu đã duyệt.

![Figure 37. Execution thành công](/images/5-Workshop/5.4-Edms-deployment/execution-succeeded.png)

> **Ghi chú:** Định nghĩa ASL đầy đủ nằm trong repository (`backend/template.yaml`, resource `DocumentApprovalStateMachine`). Nó dùng `waitForTaskToken` để workflow có thể chờ quyết định của manager.
