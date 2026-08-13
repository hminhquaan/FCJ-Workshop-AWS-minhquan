---
title: "Worklog Tuần 7"
date: 2024-01-01
weight: 7
chapter: false
pre: " <b> 1.7. </b> "
---

### Mục tiêu Tuần 7:

* Đóng gói backend thành Lambda (AWS SAM).
* Thiết lập CI/CD pipeline với GitHub Actions + OIDC.
* Deploy backend và xác minh API.

### Nhiệm vụ trong tuần:

| STT | Nhiệm vụ | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo |
| --- | --- | --- | --- | --- |
| 1 | - Đóng gói backend Spring Boot thành fat jar | 03/08/2026 | 03/08/2026 | |
| 2 | - Viết SAM template (`template.yaml`) cho Lambda + API Gateway | 04/08/2026 | 04/08/2026 | |
| 3 | - Cấu hình OIDC deploy role và GitHub secrets | 05/08/2026 | 05/08/2026 | |
| 4 | - Viết GitHub Actions workflow (test, build, deploy) | 05/08/2026 | 06/08/2026 | |
| 5 | - Deploy backend qua SAM và sửa các lỗi | 06/08/2026 | 08/08/2026 | |
| 6 | - Xác minh API đã deploy (login, documents, approval) | 08/08/2026 | 09/08/2026 | |

### Kết quả Tuần 7:

* Đóng gói backend Spring Boot thành fat jar cho Lambda.
* Viết AWS SAM template cho Lambda + API Gateway.
* Cấu hình OIDC deploy role và GitHub secrets (không dùng AWS key dài hạn).
* Viết GitHub Actions workflow với các job test, build, deploy.
* Deploy backend qua SAM lên stack `edms-lambda-stack`.
* Xác minh API đã deploy end-to-end.
