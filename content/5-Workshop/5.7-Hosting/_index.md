---
title : "Hosting (Amplify) & Cleanup"
date : 2024-01-01
weight : 7
chapter : false
pre : " <b> 5.7. </b> "
---

In this final section you will host the **React frontend** with **AWS Amplify**, configure CI/CD, and then clean up the resources.

#### Host the frontend with Amplify

**AWS Amplify** hosts the React SPA over HTTPS and connects to your Git repository so that every push triggers a rebuild.

1. Open the **Amplify console** → **All apps** → **New app** → **Host web app**.
2. Connect your **Git provider** and select the EDMS repository and the `main` branch.

![Amplify connect](/images/5-Workshop/5.7-Hosting/amplify-connect.png)

3. In **Build settings**, make sure the build spec points to the frontend:
```yaml
version: 1
applications:
  - frontend:
      phases:
        preBuild:
          commands:
            - cd frontend && npm ci
        build:
          commands:
            - cd frontend && npm run build
      artifacts:
        baseDirectory: frontend/build
        files:
          - '**/*'
```

4. Add the environment variables the frontend needs:

```
REACT_APP_API_URL=<API Gateway invoke URL>
REACT_APP_COGNITO_USER_POOL_ID=<UserPoolId>
REACT_APP_COGNITO_CLIENT_ID=<ClientId>
REACT_APP_COGNITO_REGION=ap-southeast-1
```

![Amplify env](/images/5-Workshop/5.7-Hosting/amplify-env.png)

5. Choose **Save and deploy**. Amplify builds and serves your app.

![Amplify deployed](/images/5-Workshop/5.7-Hosting/amplify-deployed.png)

> **Note:** The **default domain** is the public URL of your application, for example `https://main.d3xxxxxxxx.amplifyapp.com`.

#### CI/CD (GitHub Actions + AWS SAM)

In production, deploying is fully automated. A **GitHub Actions** workflow runs on every push to `main`:

1. `test-backend` → runs `mvn test`
2. `build-frontend` → runs `npm ci && npm run build`
3. `deploy` → authenticates via **AWS STS (OIDC)** and runs `sam deploy`

This means the entire infrastructure (API Gateway, Lambda, Step Functions, IAM) is described as **infrastructure as code** in `template.yaml` and deployed consistently every time.

![CI/CD](/images/5-Workshop/5.7-Hosting/cicd.png)

#### Test the application

1. Open the Amplify URL.
2. Sign in with a user created in **Cognito** (e.g., an ADMIN, MANAGER, or USER account).
3. Upload a document, submit it for approval, then approve it as a MANAGER — you should receive an **SNS email notification**.
4. Confirm the document status changed to `APPROVED` on the dashboard.

![App](/images/5-Workshop/5.7-Hosting/app.png)

#### Clean up

To avoid ongoing costs, delete the resources you created in this workshop:

1. **Amplify:** delete the app.
2. **Step Functions:** delete the state machine.
3. **SNS:** delete the topic.
4. **Cognito:** delete the User Pool.
5. **Lambda:** delete the function.
6. **API Gateway:** delete the API.
7. **S3:** delete the buckets (including object versions).
8. **Aurora:** **delete the cluster** (Aurora is the main source of cost).
9. **CloudFormation:** delete the stack if you created one.

![Cleanup](/images/5-Workshop/5.7-Hosting/cleanup.png)

> **Best practice:** Use **AWS Cost Explorer** and **Billing Alarms** to monitor costs during the workshop.
