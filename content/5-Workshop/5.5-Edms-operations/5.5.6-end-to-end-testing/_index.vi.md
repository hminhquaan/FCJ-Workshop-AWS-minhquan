---
title : "Kiểm thử End-to-End"
date : 2024-01-01
weight : 6
chapter : false
pre : " <b> 5.5.6 </b> "
---

Thực hiện một **kiểm thử end-to-end** qua ứng dụng web đã deploy để xác minh toàn bộ luồng hoạt động.

#### 5.5.6.1 Mở ứng dụng

1. Mở URL **Amplify** (default domain).
2. Bạn sẽ thấy trang đăng nhập EDMS.

![Figure 50. Trang đăng nhập](/images/5-Workshop/5.5-Edms-operations/login.png)

#### 5.5.6.2 Đăng nhập

1. Đăng nhập với tài khoản **USER** đã tạo trong Cognito.

![Figure 51. Đăng nhập](/images/5-Workshop/5.5-Edms-operations/signin.png)

#### 5.5.6.3 Test vòng đời tài liệu

1. **Tạo / upload** một tài liệu.
2. **Nộp** để phê duyệt (trạng thái → `PENDING`).
3. Đăng xuất và đăng nhập với vai trò **MANAGER**.
4. **Duyệt** tài liệu (trạng thái → `APPROVED`).
5. Xác nhận bạn nhận được **email thông báo SNS**.

![Figure 52. Tài liệu được duyệt](/images/5-Workshop/5.5-Edms-operations/approved.png)

> **Tiêu chí thành công:** Tài liệu đi qua `DRAFT → PENDING → APPROVED`, Step Functions execution **thành công**, và **email** được gửi đến.
