---
title : "Liên kết Lambda với API Gateway"
date : 2024-01-01
weight : 12
chapter : false
pre : " <b> 5.4.12 </b> "
---

Phần này liên kết các method của API Gateway với Lambda và **deploy** API lên một stage để có invoke URL công khai.

#### 5.4.12.1 Cấu hình Lambda integration

1. Trên method bạn đã tạo, mở **Integration Request**.
2. Xác nhận **Integration type = Lambda Function**, đúng region, và tên Lambda EDMS.
3. Bấm **Save** và cấp quyền cho API Gateway invoke Lambda (console sẽ nhắc).

![Figure 32. Lambda integration](/images/5-Workshop/5.4-Edms-deployment/lambda-integration.png)

#### 5.4.12.2 Deploy API

1. Trong API Gateway console, bấm **Deploy API**.
2. **Stage:** chọn `Prod`, hoặc tạo **New stage** tên `Prod`.
3. Bấm **Deploy**.

![Figure 33. Deploy API](/images/5-Workshop/5.4-Edms-deployment/deploy-api.png)

4. Sao chép **Invoke URL**, ví dụ `https://xxxx.execute-api.ap-southeast-1.amazonaws.com/Prod`.

![Figure 34. Invoke URL](/images/5-Workshop/5.4-Edms-deployment/invoke-url.png)

> **Ghi chú:** Đặt URL này làm `REACT_APP_API_URL` cho frontend và làm API base cho backend.
