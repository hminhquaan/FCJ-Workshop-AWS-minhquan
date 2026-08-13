---
title: "Workshop"
date: 2024-01-01
weight: 5
chapter: false
pre: " <b> 5. </b> "
---

# Nền tảng Cộng tác Tài liệu Doanh nghiệp (EDMS) trên AWS

## Quản lý tài liệu Serverless trên nền tảng AWS

### 5.0.1 Tổng quan Workshop

**EDMS (Enterprise Document Management System)** là một nền tảng cộng tác tài liệu cloud-native, serverless, xây dựng hoàn toàn trên AWS. Hệ thống giúp doanh nghiệp lưu trữ, quản lý phiên bản, chia sẻ và phê duyệt tài liệu qua một nền tảng web tập trung, an toàn — thay vì rải rác qua email hoặc file server nội bộ.

Trong workshop này, bạn sẽ xây dựng một EDMS **từ đầu** trên AWS, từng dịch vụ một, theo đúng kiến trúc đang chạy trong production:

- **Lưu trữ (Amazon S3)** — lưu các file tài liệu gốc.
- **Cơ sở dữ liệu (Amazon Aurora MySQL)** — lưu metadata: users, departments, documents, versions, folders, permissions, tags, shares, approval history.
- **Xác thực (Amazon Cognito)** — đăng nhập an toàn với phân quyền theo vai trò (ADMIN / MANAGER / USER).
- **Compute (AWS Lambda + API Gateway)** — backend Spring Boot (Java 17) đóng gói thành một Lambda, phơi qua REST API.
- **Workflow (AWS Step Functions + SNS)** — điều phối quy trình phê duyệt tài liệu kèm thông báo email.
- **Hosting (AWS Amplify)** — phục vụ frontend React qua HTTPS.

### 5.0.2 Nội dung Workshop

1. [Giới thiệu](5.1-Workshop-overview/)
2. [Các bước chuẩn bị](5.2-Prerequisite/)
3. [Thiết kế và Xây dựng hạ tầng EDMS trên AWS](5.3-Edms-infrastructure/)
4. [Triển khai EDMS trên AWS](5.4-Edms-deployment/)
5. [Kiểm thử, Vận hành và Triển khai liên tục](5.5-Edms-operations/)

> **Ghi chú:** Hướng dẫn cho từng dịch vụ được viết từng bước như bạn thao tác trên AWS Console. Chỗ nào cần ảnh chụp màn hình, sẽ để **Figure** placeholder để bạn tự chụp ảnh minh chứng setup của mình trên nền tảng AWS.
