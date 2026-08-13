---
title : "Xác thực (Cognito)"
date : 2024-01-01
weight : 4
chapter : false
pre : " <b> 5.4. </b> "
---

Trong phần này bạn sẽ thiết lập **Amazon Cognito** cho xác thực và phân quyền theo vai trò.

#### Tạo User Pool

1. Mở **Cognito console** → **User pools**.
2. Chọn **Create user pool**.

![Create user pool](/images/5-Workshop/5.4-Auth/cognito-create.png)

3. Trong **Configure sign-in experience**:
+ **Sign-in options:** chọn **Email** (người dùng đăng nhập bằng email và mật khẩu).
+ Chọn **Next**.

4. Trong **Configure security requirements**:
+ Đặt **password policy** (ví dụ tối thiểu 8 ký tự có số và ký tự đặc biệt).
+ Chọn **Next**.

5. Trong **Configure sign-up experience**:
+ Giữ **Self-service sign-up** bật, hoặc tắt nếu chỉ admin tạo người dùng.
+ Chọn **Next**.

6. Trong **Configure message delivery**:
+ Chọn **Send email with Cognito** (hoặc SES nếu đã cấu hình).
+ Chọn **Next**.

7. Trong **Integrate your app**:
+ **User pool name:** `edms-user-pool`.
+ **App client name:** `edms-client` — bỏ chọn "Generate a client secret" (app client cho web/mobile không cần secret).
+ Chọn **Next**.

8. Xem lại và chọn **Create user pool**.

![User pool created](/images/5-Workshop/5.4-Auth/cognito-created.png)

> **Ghi chú:** Ghi lại **User Pool ID** và **Client ID** — backend và frontend cần chúng.

#### Tạo các nhóm (vai trò)

EDMS có ba vai trò: **ADMIN**, **MANAGER**, **USER**. Tạo chúng thành các **group** trong User Pool:

1. Trong User Pool, mở tab **Groups**.
2. Chọn **Create group** và tạo ba nhóm:

+ `ADMIN`
+ `MANAGER`
+ `USER`

![Groups](/images/5-Workshop/5.4-Auth/groups.png)

3. Gán người dùng vào các nhóm. Người dùng trong một nhóm sẽ kế thừa vai trò của nhóm đó trong ứng dụng.

> **Ghi chú:** Backend ánh xạ một Cognito group thành vai trò ứng dụng (`ADMIN` / `MANAGER` / `USER`). JWT trả về sau đăng nhập được Lambda xác thực trên mỗi request.

#### Test đăng nhập

Bạn có thể test đăng nhập bằng AWS CLI:

```bash
aws cognito-idp initiate-auth \
  --auth-flow USER_PASSWORD_AUTH \
  --client-id <CLIENT_ID> \
  --auth-parameters USERNAME=<email>,PASSWORD=<password>
```

Kết quả thành công trả về `AccessToken` và `IdToken`. Token này là thứ frontend gửi đến API.

![Sign in](/images/5-Workshop/5.4-Auth/signin.png)
