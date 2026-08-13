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

![architecture](/images/5-Workshop/5.1-Workshop-overview/diagram1.png)

#### Content

1. [Workshop overview](5.1-Workshop-overview)
2. [Prerequisites](5.2-Prerequiste/)
3. [Storage & Database](5.3-Storage-db/)
4. [Authentication (Cognito)](5.4-Auth/)
5. [Compute (Lambda & API Gateway)](5.5-Compute/)
6. [Approval Workflow (Step Functions & SNS)](5.6-Approval/)
7. [Hosting (Amplify) & Cleanup](5.7-Hosting/)

> **Note:** Instructions for a service are written in a generic way so you can follow along on the AWS Console. Where a screenshot is needed, a placeholder is left for you to capture your own setup on the AWS platform.
