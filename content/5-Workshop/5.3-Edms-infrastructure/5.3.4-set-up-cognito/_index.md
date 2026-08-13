---
title : "Initialize and Configure Cognito"
date : 2024-01-01
weight : 4
chapter : false
pre : " <b> 5.3.4 </b> "
---

Amazon Cognito provides authentication and role-based authorization for EDMS. Users sign in and receive a **JWT** that the backend validates.

#### 5.3.4.1 Create a User Pool

1. Open the **Cognito console** → **User pools**.
2. Click **Create user pool**.

![Figure 11. Create user pool](/images/5-Workshop/5.3-Edms-infrastructure/create-userpool.png)

3. In **Configure sign-in experience**:
+ **Sign-in options:** select **Email**.
+ Click **Next**.
4. In **Configure security requirements**:
+ Set a password policy (minimum 8 characters, include a number).
+ Click **Next**.
5. In **Configure sign-up experience**:
+ Keep **Self-service sign-up** as you prefer.
+ Click **Next**.
6. In **Configure message delivery**:
+ Select **Send email with Cognito**.
+ Click **Next**.
7. In **Integrate your app**:
+ **User pool name:** `edms-user-pool`.
+ **App client name:** `edms-client`; leave **Generate a client secret** unchecked.
+ Click **Next**, review, and click **Create user pool**.

![Figure 12. User pool created](/images/5-Workshop/5.3-Edms-infrastructure/userpool-created.png)

#### 5.3.4.2 Create groups (roles)

1. In your User Pool, open the **Groups** tab.
2. Click **Create group** and create three groups:
+ `ADMIN`
+ `MANAGER`
+ `USER`

![Figure 13. Groups](/images/5-Workshop/5.3-Edms-infrastructure/groups.png)

3. Assign users to groups. Users in a group inherit the group's role in the application.

#### 5.3.4.3 Note the IDs

Note down the following values — the backend and frontend need them:

```
COGNITO_USER_POOL_ID=<pool-id>     e.g. ap-southeast-1_XXXXX
COGNITO_CLIENT_ID=<client-id>
COGNITO_REGION=ap-southeast-1
```

> **Note:** The backend maps a Cognito group to an application role (`ADMIN` / `MANAGER` / `USER`) and validates the JWT on every request.
