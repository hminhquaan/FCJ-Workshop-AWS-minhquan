---
title: "Week 7 Worklog"
date: 2024-01-01
weight: 7
chapter: false
pre: " <b> 1.7. </b> "
---
{{% notice warning %}} 
⚠️ **Note:** The following information is for reference purposes only. Please **do not copy verbatim** for your own report, including this warning.
{{% /notice %}}

### Week 7 Objectives:

* Package the backend as a Lambda (AWS SAM).
* Set up the CI/CD pipeline with GitHub Actions + OIDC.
* Deploy the backend and verify the API.

### Tasks to be carried out this week:

| No. | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| 1 | - Package the Spring Boot backend as a fat jar | 03/08/2026 | 03/08/2026 | |
| 2 | - Write the SAM template (`template.yaml`) for Lambda + API Gateway | 04/08/2026 | 04/08/2026 | |
| 3 | - Configure the OIDC deploy role and GitHub secrets | 05/08/2026 | 05/08/2026 | |
| 4 | - Write the GitHub Actions workflow (test, build, deploy) | 05/08/2026 | 06/08/2026 | |
| 5 | - Deploy the backend via SAM and fix issues | 06/08/2026 | 08/08/2026 | |
| 6 | - Verify the deployed API (login, documents, approval) | 08/08/2026 | 09/08/2026 | |

### Week 7 Achievements:

* Packaged the Spring Boot backend as a fat jar for Lambda.
* Wrote the AWS SAM template for Lambda + API Gateway.
* Configured the OIDC deploy role and GitHub secrets (no long-term AWS keys).
* Wrote the GitHub Actions workflow with test, build, and deploy jobs.
* Deployed the backend via SAM to the `edms-lambda-stack`.
* Verified the deployed API end-to-end.
