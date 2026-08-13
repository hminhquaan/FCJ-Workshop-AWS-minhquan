---
title : "Hosting (Amplify) & Dọn dẹp"
date : 2024-01-01
weight : 7
chapter : false
pre : " <b> 5.7. </b> "
---

Trong phần cuối này bạn sẽ host **frontend React** bằng **AWS Amplify**, cấu hình CI/CD, rồi dọn dẹp tài nguyên.

#### Host frontend bằng Amplify

**AWS Amplify** host React SPA qua HTTPS và kết nối với Git repository để mỗi lần push tự động rebuild.

1. Mở **Amplify console** → **All apps** → **New app** → **Host web app**.
2. Kết nối **Git provider** và chọn repository EDMS với nhánh `main`.

![Amplify connect](/images/5-Workshop/5.7-Hosting/amplify-connect.png)

3. Trong **Build settings**, đảm bảo build spec trỏ đúng frontend:
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

4. Thêm các biến môi trường frontend cần:

```
REACT_APP_API_URL=<API Gateway invoke URL>
REACT_APP_COGNITO_USER_POOL_ID=<UserPoolId>
REACT_APP_COGNITO_CLIENT_ID=<ClientId>
REACT_APP_COGNITO_REGION=ap-southeast-1
```

![Amplify env](/images/5-Workshop/5.7-Hosting/amplify-env.png)

5. Chọn **Save and deploy**. Amplify build và phục vụ ứng dụng của bạn.

![Amplify deployed](/images/5-Workshop/5.7-Hosting/amplify-deployed.png)

> **Ghi chú:** **Default domain** là URL công khai của ứng dụng, ví dụ `https://main.d3xxxxxxxx.amplifyapp.com`.

#### CI/CD (GitHub Actions + AWS SAM)

Trong production, việc deploy hoàn toàn tự động. Một **GitHub Actions** workflow chạy mỗi lần push lên `main`:

1. `test-backend` → chạy `mvn test`
2. `build-frontend` → chạy `npm ci && npm run build`
3. `deploy` → xác thực qua **AWS STS (OIDC)** và chạy `sam deploy`

Điều này nghĩa là toàn bộ hạ tầng (API Gateway, Lambda, Step Functions, IAM) được mô tả là **infrastructure as code** trong `template.yaml` và deploy nhất quán mỗi lần.

![CI/CD](/images/5-Workshop/5.7-Hosting/cicd.png)

#### Test ứng dụng

1. Mở URL Amplify.
2. Đăng nhập với một user đã tạo trong **Cognito** (ví dụ tài khoản ADMIN, MANAGER, hoặc USER).
3. Tải lên một tài liệu, nộp để phê duyệt, rồi duyệt nó với vai trò MANAGER — bạn sẽ nhận **email thông báo SNS**.
4. Xác nhận trạng thái tài liệu đổi thành `APPROVED` trên dashboard.

![App](/images/5-Workshop/5.7-Hosting/app.png)

#### Dọn dẹp

Để tránh chi phí phát sinh, hãy xóa các tài nguyên đã tạo trong workshop:

1. **Amplify:** xóa app.
2. **Step Functions:** xóa state machine.
3. **SNS:** xóa topic.
4. **Cognito:** xóa User Pool.
5. **Lambda:** xóa function.
6. **API Gateway:** xóa API.
7. **S3:** xóa các bucket (kể cả object versions).
8. **Aurora:** **xóa cluster** (Aurora là nguồn chi phí chính).
9. **CloudFormation:** xóa stack nếu bạn đã tạo.

![Cleanup](/images/5-Workshop/5.7-Hosting/cleanup.png)

> **Thực hành tốt:** Dùng **AWS Cost Explorer** và **Billing Alarms** để giám sát chi phí trong lúc workshop.
