---
title : "Lưu trữ & Cơ sở dữ liệu"
date : 2024-01-01
weight : 3
chapter : false
pre : " <b> 5.3. </b> "
---

Trong phần này bạn sẽ thiết lập lớp **lưu trữ** và **cơ sở dữ liệu** của EDMS.

#### Amazon S3 (lưu trữ tài liệu)

Amazon S3 dùng để lưu các file tài liệu gốc. Metadata (ai sở hữu tài liệu, trạng thái, phiên bản) được lưu riêng trong cơ sở dữ liệu.

1. Mở **S3 console**.
2. Chọn **Create bucket**.

![Create bucket](/images/5-Workshop/5.3-Storage-db/s3-create.png)

3. Trong màn hình Create bucket:
+ **Bucket name:** chọn tên duy nhất toàn cầu, ví dụ `edms-docs-bucket-<id-tai-khoan>`.
+ **Region:** `ap-southeast-1`.
+ Giữ nguyên các trường mặc định (**Block all public access** vẫn bật, vì EDMS truy cập bucket riêng tư qua signed URL).
+ Chọn **Create bucket**.

![Bucket created](/images/5-Workshop/5.3-Storage-db/s3-created.png)

> **Ghi chú:** S3 là nơi lưu file. Việc truy cập trực tiếp thực hiện qua **pre-signed URLs** để không bao giờ bật public access.

#### Amazon Aurora MySQL (cơ sở dữ liệu metadata)

EDMS dùng **Aurora MySQL** để lưu toàn bộ metadata quan hệ: users, departments, documents, versions, folders, permissions, tags, shares, và approval history.

1. Mở **RDS console**.
2. Chọn **Create database**.
3. Trong màn hình Create database:
+ **Engine options:** chọn **Amazon Aurora** tương thích **MySQL**.
+ **Capacity type:** **Serverless v2** (hoặc provisioned cho đơn giản).
+ **Cluster identifier:** `edms-cluster`.
+ **Credentials:** đặt **master username** và **master password** (nhớ chúng — backend sẽ cần).
+ Chọn **Create database**.

![Create database](/images/5-Workshop/5.3-Storage-db/aurora-create.png)

4. Đợi trạng thái cluster trở thành **Available**.

![Database available](/images/5-Workshop/5.3-Storage-db/aurora-available.png)

5. Ghi lại **cluster endpoint** và tên database — bạn sẽ dùng trong cấu hình backend.

> **Ghi chú chi phí:** Aurora tính phí ngay cả khi không sử dụng. Hãy stop hoặc xóa cluster khi hoàn thành workshop (xem 5.7).

#### Schema cơ sở dữ liệu

Backend tự động áp dụng schema bằng **Flyway** migrations (các file trong `backend/src/main/resources/db/migration`). Các bảng chính:

```
departments, users, folders, documents, document_versions,
permissions, tags, document_tags, shares, approval_histories, audit_logs
```

![Schema](/images/5-Workshop/5.3-Storage-db/schema.png)
