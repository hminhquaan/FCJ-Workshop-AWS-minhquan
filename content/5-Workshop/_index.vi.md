---
title: "Workshop"
date: 2024-01-01
weight: 5
chapter: false
pre: " <b> 5. </b> "
---

# Nền tảng Cộng tác Tài liệu Doanh nghiệp (EDMS) trên AWS

#### Tổng quan

**EDMS (Enterprise Document Management System)** là một nền tảng cộng tác tài liệu doanh nghiệp, cloud-native, xây dựng hoàn toàn trên nền serverless của AWS. Hệ thống giúp doanh nghiệp lưu trữ, quản lý phiên bản, chia sẻ và phê duyệt tài liệu qua một nền tảng web tập trung, an toàn — thay vì rải rác qua email hoặc file server nội bộ.

Trong workshop này, bạn sẽ xây dựng một EDMS **từ đầu** trên AWS, từng dịch vụ một, theo đúng kiến trúc đang chạy trong production:

+ **Lưu trữ (Amazon S3)** — lưu các file tài liệu gốc
+ **Cơ sở dữ liệu (Amazon Aurora MySQL)** — lưu metadata: users, departments, documents, versions, folders, permissions, tags, shares, approval history
+ **Xác thực (Amazon Cognito)** — đăng nhập an toàn với phân quyền theo vai trò (ADMIN / MANAGER / USER)
+ **Compute (AWS Lambda + API Gateway)** — backend Spring Boot (Java 17) đóng gói thành 1 Lambda, phơi qua REST API
+ **Workflow (AWS Step Functions + SNS)** — điều phối quy trình phê duyệt tài liệu kèm thông báo email
+ **Hosting (AWS Amplify)** — phục vụ frontend React qua HTTPS

![architecture](/images/5-Workshop/5.1-Workshop-overview/diagram1.png)

#### Nội dung

1. [Tổng quan workshop](5.1-Workshop-overview)
2. [Điều kiện tiên quyết](5.2-Prerequiste/)
3. [Lưu trữ & Cơ sở dữ liệu](5.3-Storage-db/)
4. [Xác thực (Cognito)](5.4-Auth/)
5. [Compute (Lambda & API Gateway)](5.5-Compute/)
6. [Quy trình phê duyệt (Step Functions & SNS)](5.6-Approval/)
7. [Hosting (Amplify) & Dọn dẹp](5.7-Hosting/)

> **Ghi chú:** Hướng dẫn cho từng dịch vụ được viết dạng tổng quát để bạn thao tác theo trên AWS Console. Chỗ nào cần ảnh chụp màn hình, sẽ để placeholder để bạn tự chụp ảnh minh chứng setup của mình trên nền tảng AWS.
