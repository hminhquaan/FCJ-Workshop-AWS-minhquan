---
title : "Hosting Amplify + HTTPS"
date : 2024-01-01
weight : 2
chapter : false
pre : " <b> 5.5.2 </b> "
---

**AWS Amplify** host frontend React qua HTTPS và kết nối với Git để mỗi lần push tự động rebuild.

#### 5.5.2.1 Tạo Amplify app

1. Mở **Amplify console** → **All apps** → **New app** → **Host web app**.
2. Kết nối **Git provider** và chọn repository EDMS với nhánh `main`.

![Figure 40. Amplify connect](/images/5-Workshop/5.5-Edms-operations/amplify-connect.png)

#### 5.5.2.2 Build settings

Trong **Build settings**, đảm bảo build spec trỏ đúng frontend:

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

#### 5.5.2.3 Thêm biến môi trường

Thêm các biến môi trường frontend:

```
REACT_APP_API_URL=<API Gateway invoke URL>
REACT_APP_COGNITO_USER_POOL_ID=<pool-id>
REACT_APP_COGNITO_CLIENT_ID=<client-id>
REACT_APP_COGNITO_REGION=ap-southeast-1
```

#### 5.5.2.4 Deploy

1. Bấm **Save and deploy**.
2. Amplify build và phục vụ ứng dụng của bạn qua HTTPS.

![Figure 42. Amplify deployed](/images/5-Workshop/5.5-Edms-operations/amplify-deployed.png)

> **Ghi chú:** **Default domain** (ví dụ `https://main.d3xxxx.amplifyapp.com`) là URL công khai của ứng dụng.
