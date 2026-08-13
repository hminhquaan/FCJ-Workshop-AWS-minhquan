---
title : "Link Lambda with API Gateway"
date : 2024-01-01
weight : 12
chapter : false
pre : " <b> 5.4.12 </b> "
---

This section links the API Gateway methods to the Lambda and **deploys** the API to a stage so it has a public invoke URL.

#### 5.4.12.1 Configure the Lambda integration

1. On the method you created, open **Integration Request**.
2. Confirm **Integration type = Lambda Function**, the correct region, and the EDMS Lambda function name.
3. Click **Save** and grant API Gateway permission to invoke the Lambda (the console prompts you).

![Figure 32. Lambda integration](/images/5-Workshop/5.4-Edms-deployment/lambda-integration.png)

#### 5.4.12.2 Deploy the API

1. In the API Gateway console, click **Deploy API**.
2. **Stage:** select `Prod`, or create a **New stage** named `Prod`.
3. Click **Deploy**.

![Figure 33. Deploy API](/images/5-Workshop/5.4-Edms-deployment/deploy-api.png)

4. Copy the **Invoke URL**, e.g. `https://xxxx.execute-api.ap-southeast-1.amazonaws.com/Prod`.

![Figure 34. Invoke URL](/images/5-Workshop/5.4-Edms-deployment/invoke-url.png)

> **Note:** Set this URL as `REACT_APP_API_URL` for the frontend and for the backend's public API base.
