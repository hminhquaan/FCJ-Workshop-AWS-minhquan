---
title : "Dọn dẹp Tài nguyên"
date : 2024-01-01
weight : 7
chapter : false
pre : " <b> 5.5.7 </b> "
---

Để tránh chi phí phát sinh, hãy xóa các tài nguyên đã tạo trong workshop này.

#### 5.5.7.1 Xóa SAM/CloudFormation stack

Lambda, API Gateway, Step Functions, và IAM resources do SAM tạo có thể được xóa bằng một lệnh (hoặc xóa stack trong console):

```bash
sam delete --stack-name edms-lambda-stack --no-prompts
```

![Figure 53. Xóa stack](/images/5-Workshop/5.5-Edms-operations/delete-stack.png)

#### 5.5.7.2 Xóa các tài nguyên còn lại thủ công

Xóa các mục sau trong console:

1. **Amplify** — xóa app.
2. **Step Functions** — xóa state machine.
3. **SNS** — xóa topic `edms-notifications`.
4. **Cognito** — xóa User Pool.
5. **S3** — làm trống và xóa các bucket (kể cả object versions).
6. **Aurora** — **xóa cluster** (nguồn chi phí chính).
7. **IAM** — xóa deploy role (sau khi gỡ mọi CloudFormation stack tham chiếu nó).

![Figure 54. Xóa tài nguyên](/images/5-Workshop/5.5-Edms-operations/delete-resources.png)

#### 5.5.7.2 Xác minh chi phí về 0

Dùng **Cost Explorer** để xác nhận không còn dịch vụ nào đang tính phí. Tùy chọn đặt cảnh báo ngân sách **$0** để được thông báo nếu còn sót.

> **Thực hành tốt:** Sau khi dọn dẹp, xác nhận danh sách stack và tài nguyên trống để không để lại một hóa đơn đang chạy.
