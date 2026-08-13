---
title : "Prerequisites"
date : 2024-01-01 
weight : 2 
chapter : false
pre : " <b> 5.2. </b> "
---

Before starting the workshop, make sure you have the following ready.

#### AWS account

You need an **AWS account** with a user that has permission to create the resources used in this workshop. We recommend using the region **ap-southeast-1 (Singapore)**.

> **Note:** To keep the cost as low as possible, clean up all resources at the end of the workshop (see section 5.7). Aurora costs money even when idle, so stop or delete the cluster when you are done.

#### IAM permissions

Make sure your user has permission to work with the services in this workshop. The full list includes at least:

```
s3:CreateBucket, s3:PutObject, s3:GetObject, s3:ListBucket
cognito-idp:CreateUserPool, cognito-idp:CreateUserPoolClient
lambda:CreateFunction, lambda:UpdateFunctionCode, lambda:InvokeFunction
apigateway:CreateRestApi, apigateway:CreateDeployment
rds:CreateDBCluster, rds:CreateDBInstance
states:CreateStateMachine, states:StartExecution
sns:CreateTopic, sns:Subscribe, sns:Publish
iam:CreateRole, iam:CreatePolicy, iam:AttachRolePolicy, iam:PassRole
cloudformation:CreateStack, cloudformation:UpdateStack, cloudformation:DeleteStack
amplify:CreateApp, amplify:CreateBranch
```

![IAM permissions](/images/5-Workshop/5.2-Prerequisite/iam.png)

> **Best practice:** Do **not** use the AWS root account. Create an IAM user for yourself and grant only the permissions you need (least-privilege).

#### Tools

The EDMS backend is a Spring Boot (Java 17) project deployed via **AWS SAM** and **GitHub Actions**. For this workshop you will need:

+ **JDK 17** (Amazon Corretto 17 recommended)
+ **Maven 3.8+**
+ **AWS SAM CLI** ≥ 1.100
+ **AWS CLI v2**
+ **Node.js 18+** (for the React frontend)
+ **Git** + a **GitHub** account

![Tools](/images/5-Workshop/5.2-Prerequisite/tools.png)

#### Source code

Clone the project repository:

```bash
git clone https://github.com/<your-account>/Enterprise-Document-Collaboration-Platform.git
cd Enterprise-Document-Collaboration-Platform
```

The repository contains:

```
backend/       Spring Boot backend (Java 17), SAM template.yaml
frontend/      React 18 SPA
.github/       GitHub Actions CI/CD workflow
```

![Repo](/images/5-Workshop/5.2-Prerequisite/repo.png)
