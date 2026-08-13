---
title : "Compute (Lambda & API Gateway)"
date : 2024-01-01
weight : 5
chapter : false
pre : " <b> 5.5. </b> "
---

In this section you will deploy the **backend** as a single **Lambda** function and expose it through **API Gateway**.

#### Backend overview

The backend is a **Spring Boot (Java 17)** monolith. It is packaged as a **fat jar** and runs inside a single Lambda function. API Gateway forwards REST requests to this Lambda.

![Compute](/images/5-Workshop/5.5-Compute/compute.png)

#### IAM Role for the Lambda

1. Open the **IAM console** → **Roles** → **Create role**.
2. Select **AWS service** → **Lambda**.
3. Attach a policy that allows at least:
+ `s3:PutObject`, `s3:GetObject`, `s3:DeleteObject`, `s3:ListBucket`
+ `cognito-idp:InitiateAuth`, `cognito-idp:GetUser`
+ `sns:Publish`
+ `states:StartExecution`, `states:SendTaskSuccess`, `states:SendTaskFailure`
+ `logs:CreateLogGroup`, `logs:CreateLogStream`, `logs:PutLogEvents`

![Lambda role](/images/5-Workshop/5.5-Compute/lambda-role.png)

4. Name the role, for example `edms-lambda-role`, and create it.

#### Create the Lambda function

1. Open the **Lambda console** → **Functions** → **Create function**.
2. Choose **Upload a .zip or .jar file**.
3. **Runtime:** `Java 17`, **Architecture:** `x86_64`.
4. **Handler:** `com.edms.StreamLambdaHandler::handleRequest`.
5. **Permissions:** choose the role `edms-lambda-role` you just created.
6. Upload the built jar (`backend-java-1.0.0-SNAPSHOT.jar`), then choose **Create function**.

![Create function](/images/5-Workshop/5.5-Compute/lambda-create.png)

7. Set the environment variables the backend needs:

```
SPRING_PROFILES_ACTIVE=aws
COGNITO_USER_POOL_ID=<UserPoolId>
COGNITO_CLIENT_ID=<ClientId>
AURORA_ENDPOINT=<Aurora cluster endpoint>
DB_USER_AWS=<db user>
DB_PASS_AWS=<db password>
AWS_S3_BUCKET=<S3 bucket name>
SNS_TOPIC_ARN=<SNS topic ARN>
STEP_FUNCTIONS_ARN=<Step Functions ARN>
```

![Environment variables](/images/5-Workshop/5.5-Compute/env.png)

> **Best practice:** Do **not** put secrets in the function code. The Lambda role carries the AWS credentials; the database password is passed as an environment variable (in production, prefer AWS Secrets Manager).

#### Expose the API via API Gateway

1. Open the **API Gateway console** → **Create API** → choose **REST API**.
2. **Protocol:** REST, **Create new API**, name it `edms-api`.
3. Create a resource with the **ANY** method and **proxy** integration pointing to the Lambda function.
4. Create a catch-all path `/{proxy+}` that also proxies to the Lambda.

![API Gateway](/images/5-Workshop/5.5-Compute/apigw.png)

5. **Deploy the API:** choose **Deploy API** → **New stage** → name it `Prod`.
6. Note the **Invoke URL** — this is the endpoint the frontend calls.

![Deployed API](/images/5-Workshop/5.5-Compute/apigw-deploy.png)

> **Note:** In production this is done by **AWS SAM** (`template.yaml`) automatically. API Gateway + Lambda + IAM role + permissions are all defined as infrastructure as code and deployed via CI/CD.

#### Test the API

With the backend running, you can test the login endpoint:

```bash
curl -X POST https://<invoke-url>/auth/login \
  -H "Content-Type: application/json" \
  -d '{"email":"admin@edms.vn","password":"your-password"}'
```

A successful call returns a token along with the user's role.

![API test](/images/5-Workshop/5.5-Compute/apigw-test.png)
