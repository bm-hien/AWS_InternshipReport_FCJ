---
title : "Tích hợp aws x-ray"
weight: 7
chapter: false
pre: " <b> 5.2.7. </b> "
---


### Mục tiêu
Sử dụng AWS X-Ray để theo dõi và kiểm thử toàn bộ luồng xử lý của API Gateway → Lambda → DynamoDB.  
X-Ray giúp quan sát trace, latency, lỗi, và các đoạn (segment/subsegment) để đảm bảo API hoạt động đúng và tối ưu.




---

### Các bước thực hiện




#### **1. Truy cập dịch vụ IAM**
- Vào **AWS Console → IAM**  


![XRAY\_1](/images/5-Workshop/2.prerequisite/iam_1.png)



- Chọn **Roles → LambdaAPIAccessRole**  




![XRAY\_2](/images/5-Workshop/3.api-gateway/3.7/xray_1.png)




- Chọn **Add permissions → Attach policies → AWSXRayDaemonWriteAccess**




![XRAY\_3](/images/5-Workshop/3.api-gateway/3.7/xray_2.png)
---




#### **2. Truy cập Lambda**
- Vào **AWS Console → Lambda**  


![XRAY\_4](/images/5-Workshop/3.api-gateway/3.7/xray_3.png)



- Chọn **Functions → DynamoDB_API_Handler**
-**Configuration → Monitoring and operations tools  → Additional monitoring tools → Edit**
![XRAY\_5](/images/5-Workshop/3.api-gateway/3.7/xray_4.png) 
- **Lambda service traces**: Tick **Enable**  




![XRAY\_6](/images/5-Workshop/3.api-gateway/3.7/xray_5.png) 










---




#### **3. Truy cập dịch vụ Api gateway**
- Vào **AWS Console → Api gateway**  


![XRAY\_7](/images/5-Workshop/3.api-gateway/3.7/xray_12.png) 



- Chọn **APIs → FlyoraAPI** 
- **Stages → Logs and tracing → Edit**
![XRAY\_8](/images/5-Workshop/3.api-gateway/3.7/xray_6.png) 
- Tick **X-Ray tracing**




![XRAY\_9](/images/5-Workshop/3.api-gateway/3.7/xray_7.png) 





---




#### **4. Kiểm thử**
- Vào **AWS Console → Lambda**
![XRAY\_10](/images/5-Workshop/3.api-gateway/3.7/xray_8.png) 
- Ở mục Test, **Create new event**
- Event name: **test**
- Dán mục dưới đây vào Event JSON:
   ```
   {
  "resource": "/{myProxy+}",
  "path": "/api/v1/bird-types",
  "httpMethod": "GET",
  "headers": {},
  "multiValueHeaders": {},
  "queryStringParameters": {},
  "multiValueQueryStringParameters": {},
  "pathParameters": {},
  "stageVariables": {},
  "requestContext": {
  "identity": {}
  },
  "body": null,
  "isBase64Encoded": false
  }
- **Save -> Test**






- Tiếp theo vào **AWS Console → X-ray**
![XRAY\_11](/images/5-Workshop/3.api-gateway/3.7/xray_9.png) 
- Ở mục **Traces**, Xuất hiện id mới
![XRAY\_12](/images/5-Workshop/3.api-gateway/3.7/xray_10.png) 
- Bấm vào, xem chi tiết các trace đó




![XRAY\_13](/images/5-Workshop/3.api-gateway/3.7/xray_11.png) 













