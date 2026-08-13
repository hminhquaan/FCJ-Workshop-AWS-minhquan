---
title : "Prepare Source Code and Project Structure"
date : 2024-01-01
weight : 1
chapter : false
pre : " <b> 5.4.1 </b> "
---

EDMS is stored in a Git repository with a **backend** (Spring Boot), a **frontend** (React), and a **CI/CD** workflow.

#### 5.4.1.1 Clone the repository

```bash
git clone https://github.com/<your-account>/Enterprise-Document-Collaboration-Platform.git
cd Enterprise-Document-Collaboration-Platform
```

#### 5.4.1.2 Project structure

The repository layout:

```
Enterprise-Document-Collaboration-Platform/
├── backend/
│   ├── pom.xml                    # Maven build (Java 17)
│   ├── template.yaml              # AWS SAM - infrastructure as code
│   └── src/main/java/com/edms/    # Spring Boot backend
│   └── src/main/resources/        # config + Flyway migrations + seed data
├── frontend/
│   └── src/                       # React 18 SPA
├── .github/workflows/deploy.yml   # CI/CD pipeline
└── .env                           # local-only config (gitignored)
```

![Figure 14. Project structure](/images/5-Workshop/5.4-Edms-deployment/project-structure.png)

#### 5.4.1.3 Backend build

Build the backend fat jar locally to verify it compiles:

```bash
cd backend
mvn clean package -DskipTests
```

The output jar `backend-java-1.0.0-SNAPSHOT.jar` is what gets deployed to Lambda.

> **Note:** The backend is a **monolith** Spring Boot application packaged as a single Lambda. `template.yaml` defines how it is deployed.
