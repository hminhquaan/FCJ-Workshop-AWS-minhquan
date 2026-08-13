---
title : "Tạo GitHub Environment 'production'"
date : 2024-01-01
weight : 6
chapter : false
pre : " <b> 5.4.6 </b> "
---

Một **GitHub Environment** nhóm các cấu hình theo môi trường và có thể yêu cầu **phê duyệt** trước khi deploy. Tạo một môi trường tên `production`.

#### 5.4.6.1 Tạo environment

1. Mở repository của bạn trên GitHub → **Settings** → **Environments**.
2. Bấm **New environment**, đặt tên `production`, rồi bấm **Configure environment**.

![Figure 20. Tạo environment](/images/5-Workshop/5.4-Edms-deployment/environment.png)

3. Tùy chọn, thêm **required reviewers** để deploy lên production cần phê duyệt thủ công.

#### 5.4.6.2 Tham chiếu environment trong workflow

Job deploy có thể dùng environment để chặn deploy:

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

> **Ghi chú:** Secret lưu ở cấp **environment** chỉ khả dụng cho các job khai báo môi trường đó. Để đơn giản, bạn có thể giữ secret ở cấp repository.
