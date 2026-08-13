---
title : "Chuẩn bị Mã nguồn và Cấu trúc Project"
date : 2024-01-01
weight : 1
chapter : false
pre : " <b> 5.4.1 </b> "
---

EDMS được lưu trong một Git repository gồm **backend** (Spring Boot), **frontend** (React), và **CI/CD** workflow.

#### 5.4.1.1 Clone repository

```bash
git clone https://github.com/<account-cua-ban>/Enterprise-Document-Collaboration-Platform.git
cd Enterprise-Document-Collaboration-Platform
```

#### 5.4.1.2 Cấu trúc project

Bố cục repository:

```
Enterprise-Document-Collaboration-Platform/
├── backend/
│   ├── pom.xml                    # Maven build (Java 17)
│   ├── template.yaml              # AWS SAM - infrastructure as code
│   └── src/main/java/com/edms/    # Backend Spring Boot
│   └── src/main/resources/        # config + Flyway migrations + seed data
├── frontend/
│   └── src/                       # React 18 SPA
├── .github/workflows/deploy.yml   # CI/CD pipeline
└── .env                           # config local-only (gitignored)
```

![Figure 14. Cấu trúc project](/images/5-Workshop/5.4-Edms-deployment/project-structure.png)

#### 5.4.1.3 Build backend

Build fat jar backend ở local để xác minh biên dịch:

```bash
cd backend
mvn clean package -DskipTests
```

File jar `backend-java-1.0.0-SNAPSHOT.jar` là thứ được deploy lên Lambda.

> **Ghi chú:** Backend là một **monolith** Spring Boot đóng gói thành một Lambda duy nhất. `template.yaml` định nghĩa cách nó được deploy.
