---
title : "Khởi tạo và Cấu hình S3"
date : 2024-01-01
weight : 1
chapter : false
pre : " <b> 5.3.1 </b> "
---

Amazon S3 lưu trữ các file tài liệu gốc. Vì EDMS truy cập file qua **pre-signed URLs**, bucket được tạo ở chế độ private (không public).

#### 5.3.1.1 Tạo S3 bucket

1. Mở **Amazon S3 console**.
2. Bấm **Create bucket**.

![Figure 1. Tạo bucket](/images/5-Workshop/5.3-Edms-infrastructure/create-bucket.png)

3. Trong trang **Create bucket**:
+ **Bucket name:** tên duy nhất toàn cầu, ví dụ `edms-docs-bucket-<id-tai-khoan>`.
+ **AWS Region:** `ap-southeast-1`.
+ Trong **Object Ownership**: giữ **ACLs disabled**.
+ Trong **Block Public Access settings for this bucket**: giữ cả bốn ô **được tick** (bucket phải ở chế độ private).
+ **Bucket Versioning:** bật (hỗ trợ lịch sử phiên bản tài liệu).
+ Bấm **Create bucket**.

![Figure 2. Cấu hình bucket](/images/5-Workshop/5.3-Edms-infrastructure/bucket-config.png)

#### 5.3.1.2 Xác minh bucket

1. Trong danh sách bucket, xác nhận bucket của bạn xuất hiện.
2. Mở bucket — nó phải trống và **private**.

![Figure 3. Bucket đã tạo](/images/5-Workshop/5.3-Edms-infrastructure/bucket-created.png)

#### 5.3.1.3 Ghi lại tên bucket

Backend cần tên bucket để lưu và lấy file. Ghi lại — bạn sẽ đưa nó vào cấu hình `.env` / SAM sau này.

> **Ghi chú:** Với workshop này chúng ta giữ bucket ở chế độ private và dùng pre-signed URLs để truy cập. **Không** bật public access.
