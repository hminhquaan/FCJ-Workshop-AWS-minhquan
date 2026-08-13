---
title : "Configure GitHub Actions Workflow"
date : 2024-01-01
weight : 2
chapter : false
pre : " <b> 5.4.2 </b> "
---

The CI/CD pipeline is defined in `.github/workflows/deploy.yml`. It runs on every push to the `main` branch.

#### 5.4.2.1 Workflow overview

The workflow has three jobs:

1. `test-backend` — runs `mvn test`
2. `build-frontend` — runs `npm ci && npm run build`
3. `deploy` — authenticates via OIDC and runs `sam deploy`

#### 5.4.2.2 The deploy.yml file

```yaml
name: EDMS CI/CD

on:
  push:
    branches: [main]

permissions:
  id-token: write   # needed for OIDC
  contents: read

jobs:
  test-backend:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-java@v4
        with:
          distribution: corretto
          java-version: "17"
      - run: mvn -B test
        working-directory: backend

  build-frontend:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: "18"
      - run: npm ci && npm run build
        working-directory: frontend

  deploy:
    needs: [test-backend, build-frontend]
    if: github.ref == 'refs/heads/main'
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Configure AWS credentials (OIDC)
        uses: aws-actions/configure-aws-credentials@v4
        with:
          role-to-assume: ${{ secrets.AWS_DEPLOY_ROLE_ARN }}
          aws-region: ap-southeast-1
      - run: mvn -B clean package -DskipTests
        working-directory: backend
      - uses: aws-actions/setup-sam@v2
      - name: Deploy via SAM
        working-directory: backend
        run: |
          sam deploy --stack-name edms-lambda-stack \
            --no-confirm-changeset --no-fail-on-empty-changeset \
            --capabilities CAPABILITY_IAM CAPABILITY_AUTO_EXPAND \
            --parameter-overrides "CognitoUserPoolId=${{ secrets.COGNITO_USER_POOL_ID }} CognitoClientId=${{ secrets.COGNITO_CLIENT_ID }} AuroraEndpoint=${{ secrets.AURORA_ENDPOINT }} S3BucketName=${{ secrets.AWS_S3_BUCKET }} DbUserName=${{ secrets.DB_USER_AWS }} DbUserPass=${{ secrets.DB_PASS_AWS }} SnsTopicArn=${{ secrets.SNS_TOPIC_ARN }} BackendLambdaArn=${{ secrets.BACKEND_LAMBDA_ARN }}"
```

> **Key point:** The deploy job uses **OIDC** (`configure-aws-credentials` with `role-to-assume`) — no long-term AWS keys are stored in GitHub.
