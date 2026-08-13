---
title: "Proposal"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 2. </b> "
---
{{% notice warning %}}
⚠️ **Note:** The information below is for reference purposes only. Please **do not copy verbatim** for your report, including this warning.
{{% /notice %}}

In this section, you need to summarize the contents of the workshop that you **plan** to conduct.

# Enterprise Document Collaboration Platform (EDMS)
## A Unified AWS Serverless Solution for Document Management

### 1. Executive Summary
The Enterprise Document Collaboration Platform (EDMS) is a cloud-native document management system that lets enterprises store, version, share, and approve documents through a central, secure web platform. It replaces scattered emails and local file servers with a single serverless architecture on AWS. The platform supports role-based access (ADMIN / MANAGER / USER), an automated approval workflow, and email notifications, all with near-zero operational overhead.

### 2. Problem Statement
#### What's the Problem?
Small and medium enterprises manage internal documents (contracts, HR files, department reports) in a fragmented way — across email, personal Google Drive, and on-premise file servers. This leads to: no control over who accesses which document, no approval process before a document is published, fixed infrastructure costs regardless of usage, and difficulty auditing document history.

#### The Solution
EDMS solves this with a fully serverless AWS architecture: **Amazon S3** stores files, **Amazon Aurora MySQL** stores metadata, **AWS Lambda + API Gateway** run the Spring Boot backend, **Amazon Cognito** provides authentication and role-based authorization, **AWS Step Functions** orchestrates the document approval workflow, **Amazon SNS** sends email notifications, and **AWS Amplify** hosts the React frontend. It scales automatically, charges only for actual usage, and keeps idle cost near zero.

#### Benefits and Return on Investment
The platform eliminates manual document handling, enforces an approval gate before publication, centralizes access control and audit history, and reduces infrastructure cost by scaling to zero when idle. As infrastructure as code (AWS SAM), it is reproducible and cheap to operate.

### 3. Solution Architecture
The platform employs a serverless AWS architecture:

![EDMS Architecture](/images/2-Proposal/edms_architecture.png)

1. User signs in via **Cognito** and receives a JWT token.
2. The React frontend calls **API Gateway** with the token.
3. **API Gateway** forwards requests to **Lambda** (Spring Boot), which validates the token.
4. **Lambda** reads/writes metadata in **Aurora** and stores files in **S3**.
5. On document submission, **Lambda** starts a **Step Functions** execution.
6. **Step Functions** orchestrates approval and publishes notifications via **SNS**.

#### AWS Services Used
- **Amazon S3**: Stores original document files (private, accessed via pre-signed URLs).
- **Amazon Aurora MySQL**: Stores relational metadata (users, documents, versions, permissions, approval history).
- **AWS Lambda**: Runs the Spring Boot backend monolith (Java 17).
- **Amazon API Gateway**: Exposes the backend as a REST API.
- **Amazon Cognito**: Provides authentication and role-based access (ADMIN/MANAGER/USER).
- **AWS Step Functions**: Orchestrates the document approval workflow (waitForTaskToken).
- **Amazon SNS**: Sends email notifications on approve/reject.
- **AWS Amplify**: Hosts the React frontend over HTTPS.
- **Amazon CloudWatch**: Logs and metrics.
- **AWS SAM / CloudFormation + GitHub Actions**: Infrastructure as code and CI/CD.

#### Component Design
- **Frontend**: React 18 SPA hosted on Amplify.
- **Backend**: Spring Boot (Java 17) packaged as a single Lambda.
- **Data Storage**: Aurora for metadata, S3 for files.
- **Workflow**: Step Functions with waitForTaskToken for human approval.
- **User Management**: Cognito groups map to application roles.

### 4. Technical Implementation
**Implementation Phases**
- Build Theory and Draw Architecture: Research AWS serverless and design the EDMS architecture (pre-internship).
- Set Up Infrastructure: Create S3, Aurora, Cognito, IAM roles on AWS (Weeks 1-2).
- Build Backend: Implement Spring Boot backend (auth, documents, folders, permissions, versions, tags, search) (Weeks 3-5).
- Build Workflow & CI/CD: Add Step Functions approval, SNS notifications, SAM template, GitHub Actions (Weeks 6-7).
- Deploy & Test: Deploy backend via SAM, host frontend on Amplify, run end-to-end tests (Week 8).

**Technical Requirements**
- Java 17 (Spring Boot), Spring Data JPA, Spring Security, AWS SDK v2.
- AWS services: S3, Aurora, Lambda, API Gateway, Cognito, Step Functions, SNS, Amplify, CloudWatch.
- React 18 for the frontend.
- AWS SAM + GitHub Actions for deployment.

### 5. Timeline & Milestones
- **Week 1-2**: AWS account setup, create S3/Aurora/Cognito/IAM.
- **Week 3-5**: Build backend core features.
- **Week 6-7**: Approval workflow (Step Functions + SNS) and CI/CD.
- **Week 8**: Deploy, host frontend, and end-to-end test.

### 6. Budget Estimation
As a serverless architecture, EDMS costs only for actual usage:
- AWS Lambda: pay per invocation.
- Amazon S3: pay per GB stored.
- Amazon Aurora: only source of steady cost — stop/delete when idle.
- Amplify, API Gateway, Cognito, SNS, Step Functions: free tier or near-zero.

> Use the AWS Pricing Calculator to estimate your specific usage. Aurora is the main cost driver, so it should be stopped or deleted when not in use.

### 7. Risk Assessment
#### Risk Matrix
- Aurora cost overruns: Medium impact, medium probability.
- Cold start latency on Java Lambda: Medium impact, low probability.
- Approval workflow failures: Low impact, low probability.

#### Mitigation Strategies
- Cost: budget alerts, stop/delete Aurora when idle.
- Cold start: warm-up invocation or provisioned concurrency.
- Workflow: Step Functions retries and CloudWatch monitoring.

### 8. Expected Outcomes
#### Technical Improvements
A centralized, secure document platform with approval workflow and audit history, replacing manual processes.
#### Long-term Value
Reusable serverless architecture, infrastructure as code, and a foundation for further feature development.
