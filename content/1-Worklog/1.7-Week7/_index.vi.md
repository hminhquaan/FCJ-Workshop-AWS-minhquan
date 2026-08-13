---
title: "Worklog Tuáº§n 7"
date: 2024-01-01
weight: 7
chapter: false
pre: " <b> 1.7. </b> "
---

### Má»¥c tiÃªu Tuáº§n 7:

* ÄÃ³ng gÃ³i backend thÃ nh Lambda (AWS SAM).
* Thiáº¿t láº­p CI/CD pipeline vá»›i GitHub Actions + OIDC.
* Deploy backend vÃ  xÃ¡c minh API.

### Nhiá»‡m vá»¥ trong tuáº§n:

| STT | Nhiá»‡m vá»¥ | NgÃ y báº¯t Ä‘áº§u | NgÃ y hoÃ n thÃ nh | TÃ i liá»‡u tham kháº£o |
| --- | --- | --- | --- | --- |
| 1 | - ÄÃ³ng gÃ³i backend Spring Boot thÃ nh fat jar | 03/08/2026 | 03/08/2026 | |
| 2 | - Viáº¿t SAM template (`template.yaml`) cho Lambda + API Gateway | 04/08/2026 | 04/08/2026 | |
| 3 | - Cáº¥u hÃ¬nh OIDC deploy role vÃ  GitHub secrets | 05/08/2026 | 05/08/2026 | |
| 4 | - Viáº¿t GitHub Actions workflow (test, build, deploy) | 05/08/2026 | 06/08/2026 | |
| 5 | - Deploy backend qua SAM vÃ  sá»­a cÃ¡c lá»—i | 06/08/2026 | 08/08/2026 | |
| 6 | - XÃ¡c minh API Ä‘Ã£ deploy (login, documents, approval) | 08/08/2026 | 09/08/2026 | |

### Káº¿t quáº£ Tuáº§n 7:

* ÄÃ³ng gÃ³i backend Spring Boot thÃ nh fat jar cho Lambda.
* Viáº¿t AWS SAM template cho Lambda + API Gateway.
* Cáº¥u hÃ¬nh OIDC deploy role vÃ  GitHub secrets (khÃ´ng dÃ¹ng AWS key dÃ i háº¡n).
* Viáº¿t GitHub Actions workflow vá»›i cÃ¡c job test, build, deploy.
* Deploy backend qua SAM lÃªn stack `edms-lambda-stack`.
* XÃ¡c minh API Ä‘Ã£ deploy end-to-end.
