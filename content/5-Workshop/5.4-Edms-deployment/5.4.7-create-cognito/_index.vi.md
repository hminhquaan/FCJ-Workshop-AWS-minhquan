---
title : "Tạo Cognito User Pool"
date : 2024-01-01
weight : 7
chapter : false
pre : " <b> 5.4.7 </b> "
---

Cognito User Pool tạo ở 5.3.4 cung cấp xác thực. Trong phần này bạn tạo **người dùng** và gán vào các nhóm để test.

#### 5.4.7.1 Tạo user test

1. Mở **Cognito console** → User Pool của bạn → tab **Users**.
2. Bấm **Create user**.

![Figure 21. Tạo user](/images/5-Workshop/5.4-Edms-deployment/create-user.png)

3. Điền email (làm username) và đặt mật khẩu tạm.
4. Tùy chọn, tick **Mark as verified** cho email.
5. Bấm **Create user**.

#### 5.4.7.2 Gán user vào một nhóm

1. Trong chi tiết user, mở tab **Groups**.
2. Bấm **Add user to group** và chọn một trong `ADMIN`, `MANAGER`, `USER`.

![Figure 22. Gán nhóm](/images/5-Workshop/5.4-Edms-deployment/assign-group.png)

> **Ghi chú:** Tạo ít nhất một user trong mỗi nhóm (`ADMIN`, `MANAGER`, `USER`) để sau này test các hành vi vai trò khác nhau.
