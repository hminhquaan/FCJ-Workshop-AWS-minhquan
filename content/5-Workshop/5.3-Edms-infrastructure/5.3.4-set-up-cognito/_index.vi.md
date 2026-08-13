---
title : "Khởi tạo và Cấu hình Cognito"
date : 2024-01-01
weight : 4
chapter : false
pre : " <b> 5.3.4 </b> "
---

Amazon Cognito cung cấp xác thực và phân quyền theo vai trò cho EDMS. Người dùng đăng nhập và nhận **JWT** mà backend xác thực.

#### 5.3.4.1 Tạo User Pool

1. Mở **Cognito console** → **User pools**.
2. Bấm **Create user pool**.

![Figure 11. Tạo user pool](/images/5-Workshop/5.3-Edms-infrastructure/create-userpool.png)

3. Trong **Configure sign-in experience**:
+ **Sign-in options:** chọn **Email**.
+ Bấm **Next**.
4. Trong **Configure security requirements**:
+ Đặt password policy (tối thiểu 8 ký tự, có chữ số).
+ Bấm **Next**.
5. Trong **Configure sign-up experience**:
+ Giữ **Self-service sign-up** tùy ý bạn.
+ Bấm **Next**.
6. Trong **Configure message delivery**:
+ Chọn **Send email with Cognito**.
+ Bấm **Next**.
7. Trong **Integrate your app**:
+ **User pool name:** `edms-user-pool`.
+ **App client name:** `edms-client`; bỏ tick **Generate a client secret**.
+ Bấm **Next**, xem lại, rồi bấm **Create user pool**.

![Figure 12. User pool đã tạo](/images/5-Workshop/5.3-Edms-infrastructure/userpool-created.png)

#### 5.3.4.2 Tạo các nhóm (vai trò)

1. Trong User Pool, mở tab **Groups**.
2. Bấm **Create group** và tạo ba nhóm:
+ `ADMIN`
+ `MANAGER`
+ `USER`

![Figure 13. Groups](/images/5-Workshop/5.3-Edms-infrastructure/groups.png)

3. Gán người dùng vào các nhóm. Người dùng trong một nhóm kế thừa vai trò của nhóm đó trong ứng dụng.

#### 5.3.4.3 Ghi lại các ID

Ghi lại các giá trị sau — backend và frontend cần chúng:

```
COGNITO_USER_POOL_ID=<pool-id>     ví dụ ap-southeast-1_XXXXX
COGNITO_CLIENT_ID=<client-id>
COGNITO_REGION=ap-southeast-1
```

> **Ghi chú:** Backend ánh xạ một Cognito group thành vai trò ứng dụng (`ADMIN` / `MANAGER` / `USER`) và xác thực JWT trên mỗi request.
