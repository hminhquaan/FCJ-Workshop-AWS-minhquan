---
title : "Authentication (Cognito)"
date : 2024-01-01
weight : 4
chapter : false
pre : " <b> 5.4. </b> "
---

In this section you will set up **Amazon Cognito** for authentication and role-based authorization.

#### Create a User Pool

1. Open the **Cognito console** → **User pools**.
2. Choose **Create user pool**.

![Create user pool](/images/5-Workshop/5.4-Auth/cognito-create.png)

3. In the **Configure sign-in experience**:
+ **Sign-in options:** select **Email** (users sign in with email and password).
+ Choose **Next**.

4. In **Configure security requirements**:
+ Set a **password policy** (e.g., minimum 8 characters with numbers and symbols).
+ Choose **Next**.

5. In **Configure sign-up experience**:
+ Keep **Self-service sign-up** enabled, or disabled if only admins create users.
+ Choose **Next**.

6. In **Configure message delivery**:
+ Select **Send email with Cognito** (or SES if configured).
+ Choose **Next**.

7. In **Integrate your app**:
+ **User pool name:** `edms-user-pool`.
+ **App client name:** `edms-client` — leave "Generate a client secret" **unchecked** (a mobile/web app client does not need a secret).
+ Choose **Next**.

8. Review and choose **Create user pool**.

![User pool created](/images/5-Workshop/5.4-Auth/cognito-created.png)

> **Note:** Note down the **User Pool ID** and the **Client ID** — the backend and frontend need them.

#### Create groups (roles)

EDMS has three roles: **ADMIN**, **MANAGER**, **USER**. Create them as **groups** in the User Pool:

1. In your User Pool, open the **Groups** tab.
2. Choose **Create group** and create three groups:

+ `ADMIN`
+ `MANAGER`
+ `USER`

![Groups](/images/5-Workshop/5.4-Auth/groups.png)

3. Assign users to the groups. Users in a group inherit the group's role in the application.

> **Note:** The backend maps a Cognito group to an application role (`ADMIN` / `MANAGER` / `USER`). The JWT returned after sign-in is validated by the Lambda on every request.

#### Test sign-in

You can test sign-in using the AWS CLI:

```bash
aws cognito-idp initiate-auth \
  --auth-flow USER_PASSWORD_AUTH \
  --client-id <CLIENT_ID> \
  --auth-parameters USERNAME=<email>,PASSWORD=<password>
```

A successful call returns an `AccessToken` and an `IdToken`. This token is what the frontend sends to the API.

![Sign in](/images/5-Workshop/5.4-Auth/signin.png)
