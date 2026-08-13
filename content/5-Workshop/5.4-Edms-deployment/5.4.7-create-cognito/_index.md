---
title : "Create Cognito User Pool"
date : 2024-01-01
weight : 7
chapter : false
pre : " <b> 5.4.7 </b> "
---

The Cognito User Pool created in 5.3.4 provides authentication. In this section you create **users** and assign them to groups for testing.

#### 5.4.7.1 Create a test user

1. Open the **Cognito console** → your User Pool → **Users** tab.
2. Click **Create user**.

![Figure 21. Create user](/images/5-Workshop/5.4-Edms-deployment/create-user.png)

3. Fill in the email (as username) and set a temporary password.
4. Optionally, mark **Mark as verified** for the email.
5. Click **Create user**.

#### 5.4.7.2 Assign a user to a group

1. In the user details, open the **Groups** tab.
2. Click **Add user to group** and select one of `ADMIN`, `MANAGER`, `USER`.

![Figure 22. Assign group](/images/5-Workshop/5.4-Edms-deployment/assign-group.png)

> **Note:** Create at least one user in each group (`ADMIN`, `MANAGER`, `USER`) so you can test the different role behaviors later.
