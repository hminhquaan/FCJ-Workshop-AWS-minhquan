---
title: "Proposal"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 2. </b> "
---
{{% notice warning %}}
⚠️ **Ghi chú:** Thông tin dưới đây chỉ để tham khảo. Vui lòng **không sao chép nguyên văn** vào báo cáo của bạn, bao gồm cả cảnh báo này.
{{% /notice %}}

Trong phần này, bạn cần tóm tắt nội dung workshop mà bạn **dự định** thực hiện.

# Nền tảng Cộng tác Tài liệu Doanh nghiệp (EDMS)
## Giải pháp Serverless AWS hợp nhất để quản lý tài liệu

### 1. Tóm tắt điều hành
Nền tảng Cộng tác Tài liệu Doanh nghiệp (EDMS) là một hệ thống quản lý tài liệu cloud-native giúp doanh nghiệp lưu trữ, quản lý phiên bản, chia sẻ và phê duyệt tài liệu qua một nền tảng web tập trung, an toàn. Hệ thống thay thế email rời rạc và file server nội bộ bằng một kiến trúc serverless duy nhất trên AWS. Nền tảng hỗ trợ truy cập theo vai trò (ADMIN / MANAGER / USER), quy trình phê duyệt tự động, và thông báo email, với chi phí vận hành gần bằng không.

### 2. Phát biểu vấn đề
#### Vấn đề là gì?
Các doanh nghiệp vừa và nhỏ quản lý tài liệu nội bộ (hợp đồng, hồ sơ nhân sự, báo cáo phòng ban) một cách rời rạc — qua email, Google Drive cá nhân, hoặc file server on-premise. Điều này dẫn đến: không kiểm soát được ai truy cập tài liệu nào, không có quy trình phê duyệt trước khi công bố, chi phí hạ tầng cố định dù tải sử dụng không đều, và khó audit lịch sử tài liệu.

#### Giải pháp
EDMS giải quyết bằng một kiến trúc AWS hoàn toàn serverless: **Amazon S3** lưu file, **Amazon Aurora MySQL** lưu metadata, **AWS Lambda + API Gateway** chạy backend Spring Boot, **Amazon Cognito** cung cấp xác thực và phân quyền, **AWS Step Functions** điều phối quy trình phê duyệt, **Amazon SNS** gửi thông báo email, và **AWS Amplify** host frontend React. Hệ thống tự động scale, chỉ trả tiền theo mức sử dụng thực tế, và giữ chi phí idle gần bằng không.

#### Lợi ích và Hoàn vốn
Nền tảng loại bỏ việc xử lý tài liệu thủ công, áp dụng cổng phê duyệt trước khi công bố, tập trung kiểm soát truy cập và lịch sử audit, đồng thời giảm chi phí hạ tầng bằng cách scale về không khi idle. Là infrastructure as code (AWS SAM), hệ thống tái tạo được và vận hành rẻ.

### 3. Kiến trúc giải pháp
Nền tảng sử dụng kiến trúc serverless AWS:

![Kiến trúc EDMS](/images/2-Proposal/edms_architecture.png)

1. Người dùng đăng nhập qua **Cognito** và nhận JWT token.
2. Frontend React gọi **API Gateway** kèm token.
3. **API Gateway** chuyển tiếp request đến **Lambda** (Spring Boot), Lambda xác thực token.
4. **Lambda** đọc/ghi metadata trong **Aurora** và lưu file trong **S3**.
5. Khi tài liệu được nộp, **Lambda** khởi động execution của **Step Functions**.
6. **Step Functions** điều phối phê duyệt và gửi thông báo qua **SNS**.

#### AWS Services sử dụng
- **Amazon S3**: Lưu file tài liệu gốc (private, truy cập qua pre-signed URLs).
- **Amazon Aurora MySQL**: Lưu metadata quan hệ (users, documents, versions, permissions, approval history).
- **AWS Lambda**: Chạy backend monolith Spring Boot (Java 17).
- **Amazon API Gateway**: Phơi backend thành REST API.
- **Amazon Cognito**: Xác thực và phân quyền (ADMIN/MANAGER/USER).
- **AWS Step Functions**: Điều phối quy trình phê duyệt (waitForTaskToken).
- **Amazon SNS**: Gửi email khi duyệt/từ chối.
- **AWS Amplify**: Host frontend React qua HTTPS.
- **Amazon CloudWatch**: Log và metric.
- **AWS SAM / CloudFormation + GitHub Actions**: Infrastructure as code và CI/CD.

#### Thiết kế thành phần
- **Frontend**: React 18 SPA host trên Amplify.
- **Backend**: Spring Boot (Java 17) đóng gói thành một Lambda.
- **Lưu trữ dữ liệu**: Aurora cho metadata, S3 cho file.
- **Workflow**: Step Functions với waitForTaskToken cho phê duyệt con người.
- **Quản lý người dùng**: Cognito groups ánh xạ thành vai trò ứng dụng.

### 4. Triển khai kỹ thuật
**Các giai đoạn thực hiện**
- Xây lý thuyết và vẽ kiến trúc: Nghiên cứu AWS serverless và thiết kế kiến trúc EDMS (trước thực tập).
- Thiết lập hạ tầng: Tạo S3, Aurora, Cognito, IAM roles trên AWS (Tuần 1-2).
- Xây backend: Triển khai backend Spring Boot (auth, documents, folders, permissions, versions, tags, search) (Tuần 3-5).
- Xây workflow & CI/CD: Thêm Step Functions approval, SNS notifications, SAM template, GitHub Actions (Tuần 6-7).
- Deploy & test: Deploy backend qua SAM, host frontend trên Amplify, chạy end-to-end tests (Tuần 8).

**Yêu cầu kỹ thuật**
- Java 17 (Spring Boot), Spring Data JPA, Spring Security, AWS SDK v2.
- Các dịch vụ AWS: S3, Aurora, Lambda, API Gateway, Cognito, Step Functions, SNS, Amplify, CloudWatch.
- React 18 cho frontend.
- AWS SAM + GitHub Actions cho deployment.

### 5. Lộ trình & Mốc
- **Tuần 1-2**: Thiết lập AWS account, tạo S3/Aurora/Cognito/IAM.
- **Tuần 3-5**: Xây các tính năng lõi backend.
- **Tuần 6-7**: Quy trình phê duyệt (Step Functions + SNS) và CI/CD.
- **Tuần 8**: Deploy, host frontend, và test end-to-end.

### 6. Ước tính ngân sách
Là kiến trúc serverless, EDMS chỉ trả tiền theo mức sử dụng thực tế:
- AWS Lambda: trả theo số lần invoke.
- Amazon S3: trả theo GB lưu trữ.
- Amazon Aurora: nguồn chi phí ổn định duy nhất — stop/xóa khi không dùng.
- Amplify, API Gateway, Cognito, SNS, Step Functions: free tier hoặc gần bằng không.

> Dùng AWS Pricing Calculator để ước tính mức sử dụng cụ thể. Aurora là nguồn chi phí chính, nên stop hoặc xóa khi không dùng.

### 7. Đánh giá rủi ro
#### Ma trận rủi ro
- Chi phí Aurora vượt ngân sách: Tác động trung bình, xác suất trung bình.
- Cold start trên Java Lambda: Tác động trung bình, xác suất thấp.
- Lỗi quy trình phê duyệt: Tác động thấp, xác suất thấp.

#### Chiến lược giảm thiểu
- Chi phí: budget alerts, stop/xóa Aurora khi idle.
- Cold start: warm-up invocation hoặc provisioned concurrency.
- Workflow: Step Functions retries và giám sát CloudWatch.

### 8. Kết quả mong đợi
#### Cải thiện kỹ thuật
Nền tảng tài liệu tập trung, an toàn với quy trình phê duyệt và lịch sử audit, thay thế các quy trình thủ công.
#### Giá trị dài hạn
Kiến trúc serverless tái sử dụng được, infrastructure as code, và nền tảng cho việc phát triển tính năng tiếp theo.
