---
title : "Create GitHub Environment 'production'"
date : 2024-01-01
weight : 6
chapter : false
pre : " <b> 5.4.6 </b> "
---

A **GitHub Environment** groups environment-specific settings and can require **approval** before deployment. Create one named `production`.

#### 5.4.6.1 Create the environment

1. Open your repository on GitHub → **Settings** → **Environments**.
2. Click **New environment**, name it `production`, and click **Configure environment**.

3. Optionally, add **required reviewers** so that deploys to production need manual approval.

#### 5.4.6.2 Reference the environment in the workflow

The deploy job can use the environment to gate deploys:

```yaml
deploy:
  needs: [test-backend, build-frontend]
  environment: production
  runs-on: ubuntu-latest
  steps:
    - uses: actions/checkout@v4
    - name: Configure AWS credentials (OIDC)
      uses: aws-actions/configure-aws-credentials@v4
      with:
        role-to-assume: ${{ secrets.AWS_DEPLOY_ROLE_ARN }}
        aws-region: ap-southeast-1
```

> **Note:** Secrets stored at the **environment** level are only available to jobs that declare that environment. For simplicity you can keep secrets at the repository level instead.
