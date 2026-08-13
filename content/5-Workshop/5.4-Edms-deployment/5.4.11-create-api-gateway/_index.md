---
title : "Create API Gateway"
date : 2024-01-01
weight : 11
chapter : false
pre : " <b> 5.4.11 </b> "
---

**API Gateway** exposes the Lambda backend as a REST API. In production this is defined in `template.yaml` and created automatically by SAM.

#### 5.4.11.1 Create a REST API

1. Open the **API Gateway console** → **Create API**.
2. Choose **REST API** (not HTTP API) and click **Build**.

![Figure 30. Create API](/images/5-Workshop/5.4-Edms-deployment/create-api.png)

3. **Choose the protocol:** REST. **Create new API.** Name it `edms-api`.
4. Click **Create API**.

#### 5.4.11.2 Add a catch-all proxy resource

1. On the **Resources** page, create a resource with path `{proxy+}`.
2. For the `{proxy+}` resource, create an **ANY** method.
3. Set **Integration type** to **Lambda Function**, select the EDMS Lambda and region.

![Figure 31. Proxy method](/images/5-Workshop/5.4-Edms-deployment/proxy-method.png)

4. Repeat for the root `/` resource with an **ANY** method (or a `GET` for a health check).

> **Note:** The `{proxy+}` resource lets API Gateway forward any path (`/auth/login`, `/documents`, ...) to the Lambda, which routes it inside the Spring Boot app.
