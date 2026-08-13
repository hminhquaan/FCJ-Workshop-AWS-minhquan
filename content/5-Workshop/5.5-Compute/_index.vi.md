---
title : "Compute (Lambda & API Gateway)"
date : 2024-01-01
weight : 5
chapter : false
pre : " <b> 5.5. </b> "
---

Trong phần này bạn sẽ deploy **backend** thành một **Lambda** duy nhất và phơi qua **API Gateway**.

#### Tổng quan backend

Backend là một **monolith Spring Boot (Java 17)**. Nó được đóng gói thành **fat jar** và chạy bên trong một Lambda duy nhất. API Gateway chuyển tiếp các REST request đến Lambda này.

![Compute](/images/5-Workshop/5.5-Compute/compute.png)

#### IAM Role cho Lambda

1. Mở **IAM console** → **Roles** → **Create role**.
2. Chọn **AWS service** → **Lambda**.
3. Đính kèm một policy cho phép tối thiểu:
+ `s3:PutObject`, `s3:GetObject`, `s3:DeleteObject`, `s3:ListBucket`
+ `cognito-idp:InitiateAuth`, `cognito-idp:GetUser`
+ `sns:Publish`
+ `states:StartExecution`, `states:SendTaskSuccess`, `states:SendTaskFailure`
+ `logs:CreateLogGroup`, `logs:CreateLogStream`, `logs:PutLogEvents`

![Lambda role](/images/5-Workshop/5.5-Compute/lambda-role.png)

4. Đặt tên role, ví dụ `edms-lambda-role`, và tạo.

#### Tạo Lambda function

1. Mở **Lambda console** → **Functions** → **Create function**.
2. Chọn **Upload a .zip or .jar file**.
3. **Runtime:** `Java 17`, **Architecture:** `x86_64`.
4. **Handler:** `com.edms.StreamLambdaHandler::handleRequest`.
5. **Permissions:** chọn role `edms-lambda-role` vừa tạo.
6. Upload jar đã build (`backend-java-1.0.0-SNAPSHOT.jar`), rồi chọn **Create function**.

![Create function](/images/5-Workshop/5.5-Compute/lambda-create.png)

7. Đặt các biến môi trường backend cần:

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

> **Thực hành tốt:** **Không** đặt secret trong code của function. Role Lambda mang credentials AWS; mật khẩu database truyền qua biến môi trường (trong production, nên dùng AWS Secrets Manager).

#### Phơi API qua API Gateway

1. Mở **API Gateway console** → **Create API** → chọn **REST API**.
2. **Protocol:** REST, **Create new API**, đặt tên `edms-api`.
3. Tạo một resource với method **ANY** và **proxy** integration trỏ đến Lambda.
4. Tạo đường dẫn catch-all `/{proxy+}` cũng proxy đến Lambda.

![API Gateway](/images/5-Workshop/5.5-Compute/apigw.png)

5. **Deploy API:** chọn **Deploy API** → **New stage** → đặt tên `Prod`.
6. Ghi lại **Invoke URL** — đây là endpoint frontend gọi.

![Deployed API](/images/5-Workshop/5.5-Compute/apigw-deploy.png)

> **Ghi chú:** Trong production việc này được làm tự động bởi **AWS SAM** (`template.yaml`). API Gateway + Lambda + IAM role + permissions đều được định nghĩa là infrastructure as code và deploy qua CI/CD.

#### Test API

Khi backend đang chạy, bạn có thể test endpoint đăng nhập:

```bash
curl -X POST https://<invoke-url>/auth/login \
  -H "Content-Type: application/json" \
  -d '{"email":"admin@edms.vn","password":"mat-khau-cua-ban"}'
```

Kết quả thành công trả về token kèm vai trò của người dùng.

![API test](/images/5-Workshop/5.5-Compute/apigw-test.png)
