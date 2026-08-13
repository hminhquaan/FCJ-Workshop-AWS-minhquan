---
title : "Khởi tạo và Cấu hình Aurora RDS"
date : 2024-01-01
weight : 2
chapter : false
pre : " <b> 5.3.2 </b> "
---

EDMS dùng **Amazon Aurora (MySQL)** để lưu toàn bộ metadata quan hệ. Trong phần này bạn tạo Aurora cluster và ghi lại endpoint.

#### 5.3.2.1 Tạo Aurora MySQL cluster

1. Mở **Amazon RDS console** → **Databases**.
2. Bấm **Create database**.

![Figure 4. Tạo database](/images/5-Workshop/5.3-Edms-infrastructure/create-database.png)

3. Trong trang **Create database**:
+ **Engine options:** chọn **Amazon Aurora** và edition **MySQL**.
+ **Capacity type:** **Serverless v2** (khuyến nghị) hoặc **Provisioned**.
+ **DB cluster identifier:** `edms-cluster`.
+ **Credentials:** đặt **Master username** (ví dụ `admin`) và **Master password** mạnh; lưu ở nơi an toàn.
+ **Instance configuration:** nếu provisioned, chọn `db.t3.medium` (hoặc instance nhỏ).
+ **Connectivity:** chọn **Don't connect to an EC2 compute resource**.
+ Bấm **Create database**.

![Figure 5. Cấu hình Aurora](/images/5-Workshop/5.3-Edms-infrastructure/aurora-config.png)

#### 5.3.2.2 Đợi khả dụng

Cluster mất vài phút để trở thành **Available**. Đợi trạng thái đổi từ *Creating* sang *Available*.

![Figure 6. Cluster khả dụng](/images/5-Workshop/5.3-Edms-infrastructure/aurora-available.png)

#### 5.3.2.3 Tạo database ban đầu

1. Mở **Query Editor v2** hoặc kết nối bằng MySQL client.
2. Tạo database ứng dụng dùng:

```sql
CREATE DATABASE IF NOT EXISTS edms
  CHARACTER SET utf8mb4
  COLLATE utf8mb4_unicode_ci;
```

> **Ghi chú:** Backend cũng tự động áp dụng schema bằng **Flyway** migrations từ `backend/src/main/resources/db/migration`.

#### 5.3.2.4 Lấy endpoint

1. Mở cluster bạn đã tạo.
2. Chọn tab **Connectivity & security**.
3. Sao chép giá trị **Endpoint** (hostname) — ví dụ `edms-cluster.cluster-xxxx.ap-southeast-1.rds.amazonaws.com`.

![Figure 7. Endpoint](/images/5-Workshop/5.3-Edms-infrastructure/endpoint.png)

4. Lưu endpoint, user và mật khẩu database vào cấu hình `.env` / SAM của project:

```
AURORA_ENDPOINT=<endpoint>
DB_USER_AWS=admin
DB_PASS_AWS=<mat-khau>
DB_NAME=edms
```

> **Ghi chú chi phí:** Aurora tính phí ngay cả khi không sử dụng. Hãy stop hoặc xóa cluster khi hoàn thành workshop (xem 5.5.7).
