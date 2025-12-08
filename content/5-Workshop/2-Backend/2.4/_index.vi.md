---
title : "Tạo API Gateway và tích hợp với Lambda"
weight: 4
chapter: false
pre: " <b> 5.2.4. </b> "
---


### Mục tiêu
Kết nối **AWS API Gateway** với **Lambda Function** để tạo endpoint RESTful cho phép truy cập dữ liệu trong **DynamoDB**.




---




### Các bước thực hiện




#### **1. Truy cập dịch vụ API Gateway**
- Vào **AWS Console → API Gateway**  
- Chọn **Create API**




![API\_1](/images/5-Workshop/3.api-gateway/3.2/api_1.png)




- Chọn loại **REST API (Build)**  




![API\_2](/images/5-Workshop/3.api-gateway/3.2/api_2.png)




- Chọn:
  - **Create new API:** New API
  - **API name:** `FlyoraAPI`
  - **Endpoint Type:** Regional
- Chọn **Create API**




![API\_3](/images/5-Workshop/3.api-gateway/3.2/api_3.png)
---




#### **2. Tạo Resource và Method**
1. Trong sidebar, chọn **Actions → Create Resource**




![API\_4](/images/5-Workshop/3.api-gateway/3.2/api_4.png)




   - **Resource Name:** `api`
   - Chọn **Create Resource**




![API\_5](/images/5-Workshop/3.api-gateway/3.2/api_10.png)




2. Chọn **/api → Actions → Create Resource**  










3. Trong cấu hình resource:
   
   - **Resource path:** /api/  
   - **Resource Name:** `v1`
   - Nhấn **Create resource**
![API\_14](/images/5-Workshop/3.api-gateway/3.2/api_14.png)
4. Trong cấu hình resource:
   - Tick **Proxy resource**
   - **Resource path:** /api/  
   - **Resource Name:** `{proxy+}`
   - Nhấn **Create resource**
![API\_14](/images/5-Workshop/3.api-gateway/3.2/api_18.png)
5. Chọn **/v1 → Actions → Create Resource**


   Trong cấu hình resource:
   - Tick **Proxy resource**
   - **Resource path:** /api/v1/  
   - **Resource Name:** `{myProxy+}`
   - Nhấn **Create resource**

![API\_15](/images/5-Workshop/3.api-gateway/3.2/api_15.png)
6. **Enable CORS** cho tất cả resource
![API\_15](/images/5-Workshop/3.api-gateway/3.2/api_19.png)
- Vào method **OPTIONS → Integration response → Header Mappings**, đảm bảo có cấu hình: 
   - Access-Control-Allow-Origin: '*'
   - Access-Control-Allow-Headers: 'Content-Type,X-Amz-Date,Authorization,X-Api-Key,X-Amz-Security-Token'
   - Access-Control-Allow-Methods: 'DELETE,GET,HEAD,OPTIONS,PATCH,POST,PUT'
  ![API\_15](/images/5-Workshop/3.api-gateway/3.2/api_20.png)
---




#### **3. Gắn Lambda**
1. Sau khi tạo thành công **/api/vi/{myProxy+}**, xuất hiện method **ANY**:
   - Chọn **ANY → Integration request → Edit**
 




![API\_16](/images/5-Workshop/3.api-gateway/3.2/api_16.png)


1. Gắn Lambda:
   - **Integration type:** Lambda Function  
   - Tick **Lambda proxy integration**
   - **Lambda Region:** `ap-southeast-1` (Singapore)  
   - **Lambda Function:** chọn hàm `Lambda_API_Handler` của bạn
  ![API\_17](/images/5-Workshop/3.api-gateway/3.2/api_17.png)
---




#### **4. Deploy API**
- Chọn **Actions → Deploy API**
- **Deployment stage:** New stage  
  - **Stage name:** `dev`
  - **Description:** Development stage for Lambda API
- Nhấn **Deploy**




![API\_9](/images/5-Workshop/3.api-gateway/3.2/api_9.png)




Sau khi deploy, bạn sẽ nhận được Invoke URL dạng:
```https://<api_id>.execute-api.ap-southeast-1.amazonaws.com/dev```





