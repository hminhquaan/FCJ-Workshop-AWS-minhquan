---
title : "Storage & Database"
date : 2024-01-01
weight : 3
chapter : false
pre : " <b> 5.3. </b> "
---

In this section you will set up the **storage** and **database** layers of EDMS.

#### Amazon S3 (document storage)

Amazon S3 is used to store the original document files. Metadata (who owns a document, its status, its version) is stored separately in the database.

1. Open the **S3 console**.
2. Choose **Create bucket**.

![Create bucket](/images/5-Workshop/5.3-Storage-db/s3-create.png)

3. In the Create bucket console:
+ **Bucket name:** choose a globally unique name, for example `edms-docs-bucket-<your-account-id>`.
+ **Region:** `ap-southeast-1`.
+ Leave the other fields as default (**Block all public access** stays on, because EDMS accesses the bucket privately via signed URLs).
+ Choose **Create bucket**.

![Bucket created](/images/5-Workshop/5.3-Storage-db/s3-created.png)

> **Note:** S3 is where files are stored. Direct access is done via **pre-signed URLs** so that public access is never enabled.

#### Amazon Aurora MySQL (metadata database)

EDMS uses **Aurora MySQL** to store all relational metadata: users, departments, documents, versions, folders, permissions, tags, shares, and approval history.

1. Open the **RDS console**.
2. Choose **Create database**.
3. In the Create database console:
+ **Engine options:** choose **Amazon Aurora** with **MySQL** compatibility.
+ **Capacity type:** **Serverless v2** (or provisioned for simplicity).
+ **Cluster identifier:** `edms-cluster`.
+ **Credentials:** set a **master username** and **master password** (remember them — the backend needs them).
+ Choose **Create database**.

![Create database](/images/5-Workshop/5.3-Storage-db/aurora-create.png)

4. Wait for the cluster status to become **Available**.

![Database available](/images/5-Workshop/5.3-Storage-db/aurora-available.png)

5. Note the **cluster endpoint** and the database name — you will use them in the backend configuration.

> **Cost note:** Aurora charges even when idle. Stop or delete the cluster when you finish the workshop (see 5.7).

#### Database schema

The backend applies the schema automatically using **Flyway** migrations (files under `backend/src/main/resources/db/migration`). The main tables are:

```
departments, users, folders, documents, document_versions,
permissions, tags, document_tags, shares, approval_histories, audit_logs
```

![Schema](/images/5-Workshop/5.3-Storage-db/schema.png)
