---
title: "Workshop"
date: 2024-01-01
weight: 5
chapter: false
pre: " <b> 5. </b> "
---

# Enterprise Document Collaboration Platform (EDMS) on AWS

#### Overview

**EDMS (Enterprise Document Management System)** is a cloud-native, serverless document collaboration platform built entirely on AWS. It lets enterprises store, version, share, and approve documents through a central, secure, web-based platform instead of scattered emails or local file servers.

In this workshop, you will build an EDMS **from scratch** on AWS, service by service, following the same architecture that runs in production:

+ **Storage (Amazon S3)** — store the original document files
+ **Database (Amazon Aurora MySQL)** — store metadata: users, departments, documents, versions, folders, permissions, tags, shares, approval history
+ **Authentication (Amazon Cognito)** — secure sign-in with role-based access (ADMIN / MANAGER / USER)
+ **Compute (AWS Lambda + API Gateway)** — a Spring Boot (Java 17) monolith packaged as a single Lambda, exposed through a REST API
+ **Workflow (AWS Step Functions + SNS)** — orchestrate the document approval flow with email notifications
+ **Hosting (AWS Amplify)** — serve the React frontend over HTTPS

#### Content

1. [Introduction](5.1-Workshop-overview)
2. [Prerequisites](5.2-Prerequiste/)
3. [Design and Build EDMS Infrastructure on AWS](5.3-Edms-infrastructure/)
4. [Deploying EDMS on AWS](5.4-Edms-deployment/)
5. [Testing, Operations, and Continuous Deployment](5.5-Edms-operations/)

> **Note:** Instructions for each service are written step-by-step as you would perform them on the AWS Console. Where a screenshot is needed, a **Figure** placeholder is left for you to capture your own setup on the AWS platform.
