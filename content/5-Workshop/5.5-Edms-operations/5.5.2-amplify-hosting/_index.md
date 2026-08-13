---
title : "Amplify Hosting + HTTPS"
date : 2024-01-01
weight : 2
chapter : false
pre : " <b> 5.5.2 </b> "
---

**AWS Amplify** hosts the React frontend over HTTPS and connects to Git so every push triggers a rebuild.

#### 5.5.2.1 Create an Amplify app

1. Open the **Amplify console** → **All apps** → **New app** → **Host web app**.
2. Connect your **Git provider** and select the EDMS repository and the `main` branch.

![Figure 40. Amplify connect](/images/5-Workshop/5.5-Edms-operations/amplify-connect.png)

#### 5.5.2.2 Build settings

In **Build settings**, ensure the build spec targets the frontend:

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

#### 5.5.2.3 Add environment variables

Add the frontend environment variables:

```
REACT_APP_API_URL=<API Gateway invoke URL>
REACT_APP_COGNITO_USER_POOL_ID=<pool-id>
REACT_APP_COGNITO_CLIENT_ID=<client-id>
REACT_APP_COGNITO_REGION=ap-southeast-1
```

![Figure 41. Amplify env](/images/5-Workshop/5.5-Edms-operations/amplify-env.png)

#### 5.5.2.4 Deploy

1. Click **Save and deploy**.
2. Amplify builds and serves your app over HTTPS.

![Figure 42. Amplify deployed](/images/5-Workshop/5.5-Edms-operations/amplify-deployed.png)

> **Note:** The **default domain** (e.g. `https://main.d3xxxx.amplifyapp.com`) is the public URL of the application.
