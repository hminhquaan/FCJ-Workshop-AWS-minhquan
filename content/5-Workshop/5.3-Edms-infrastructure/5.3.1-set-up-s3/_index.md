---
title : "Initialize and Configure S3"
date : 2024-01-01
weight : 1
chapter : false
pre : " <b> 5.3.1 </b> "
---

Amazon S3 stores the original document files. Because EDMS accesses files via **pre-signed URLs**, the bucket is created private (no public access).

#### 5.3.1.1 Create the S3 bucket

1. Open the **Amazon S3 console**.
2. Click **Create bucket**.

![Figure 1. Create bucket](/images/5-Workshop/5.3-Edms-infrastructure/create-bucket.png)

3. In the **Create bucket** page:
+ **Bucket name:** a globally unique name, e.g. `edms-docs-bucket-<account-id>`.
+ **AWS Region:** `ap-southeast-1`.
+ Under **Object Ownership**: keep **ACLs disabled**.
+ Under **Block Public Access settings for this bucket**: keep all four boxes **checked** (the bucket must stay private).
+ **Bucket Versioning:** enable it (helps with document version history).
+ Click **Create bucket**.

![Figure 2. Bucket configuration](/images/5-Workshop/5.3-Edms-infrastructure/bucket-config.png)

#### 5.3.1.2 Verify the bucket

1. In the bucket list, confirm your bucket appears.
2. Open the bucket — it should be empty and **private**.

![Figure 3. Bucket created](/images/5-Workshop/5.3-Edms-infrastructure/bucket-created.png)

#### 5.3.1.3 Note the bucket name

The backend needs the bucket name to store and retrieve files. Note it down — you will put it in the `.env` / SAM configuration later.

> **Note:** For this workshop we keep the bucket private and use pre-signed URLs for access. Do **not** enable public access.
