---
title : "Initialize and Configure Aurora RDS"
date : 2024-01-01
weight : 2
chapter : false
pre : " <b> 5.3.2 </b> "
---

EDMS uses **Amazon Aurora (MySQL)** to store all relational metadata. In this section you create the Aurora cluster and note its endpoint.

#### 5.3.2.1 Create an Aurora MySQL cluster

1. Open the **Amazon RDS console** → **Databases**.
2. Click **Create database**.

![Figure 4. Create database](/images/5-Workshop/5.3-Edms-infrastructure/create-database.png)

3. In the **Create database** page:
+ **Engine options:** choose **Amazon Aurora** and the **MySQL** edition.
+ **Capacity type:** **Serverless v2** (recommended) or **Provisioned**.
+ **DB cluster identifier:** `edms-cluster`.
+ **Credentials:** set a **Master username** (e.g. `admin`) and a strong **Master password**; store it in a safe place.
+ **Instance configuration:** if provisioned, choose `db.t3.medium` (or a small instance).
+ **Connectivity:** choose **Don't connect to an EC2 compute resource**.
+ Click **Create database**.

![Figure 5. Aurora configuration](/images/5-Workshop/5.3-Edms-infrastructure/aurora-config.png)

#### 5.3.2.2 Wait for availability

The cluster takes several minutes to become **Available**. Wait until the status changes from *Creating* to *Available*.

![Figure 6. Cluster available](/images/5-Workshop/5.3-Edms-infrastructure/aurora-available.png)

#### 5.3.2.3 Create the initial database

1. Open the **Query Editor v2** or connect with a MySQL client.
2. Create the database used by the application:

```sql
CREATE DATABASE IF NOT EXISTS edms
  CHARACTER SET utf8mb4
  COLLATE utf8mb4_unicode_ci;
```

> **Note:** The backend also applies the schema automatically with **Flyway** migrations from `backend/src/main/resources/db/migration`.

#### 5.3.2.4 Retrieve the endpoint

1. Open the cluster you created.
2. Select the **Connectivity & security** tab.
3. Copy the **Endpoint** (hostname) value — for example `edms-cluster.cluster-xxxx.ap-southeast-1.rds.amazonaws.com`.

![Figure 7. Endpoint](/images/5-Workshop/5.3-Edms-infrastructure/endpoint.png)

4. Save the endpoint, database user, and password into the project's `.env` / SAM configuration:

```
AURORA_ENDPOINT=<endpoint>
DB_USER_AWS=admin
DB_PASS_AWS=<password>
DB_NAME=edms
```

> **Cost note:** Aurora charges even when idle. Stop or delete the cluster when you finish the workshop (see 5.5.7).
