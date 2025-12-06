---
title : "Integrating AWS X-Ray"
weight: 7
chapter: false
pre: " <b> 5.2.7. </b> "
---

### Objective
Use **AWS X-Ray** to trace and inspect the entire processing flow of **API Gateway → Lambda → DynamoDB**.  
X-Ray helps visualize traces, latency, errors, and segments/subsegments to ensure the API behaves correctly and efficiently.

---

### Implementation Steps

---

#### **1. Access IAM Service**
- Go to **AWS Console → IAM**

![XRAY_1](/images/5-Workshop/2.prerequisite/iam_1.png)

- Select **Roles → LambdaAPIAccessRole**

![XRAY_2](/images/5-Workshop/3.api-gateway/3.7/xray_1.png)

- Choose **Add permissions → Attach policies → AWSXRayDaemonWriteAccess**

![XRAY_3](/images/5-Workshop/3.api-gateway/3.7/xray_2.png)

---

#### **2. Access Lambda**
- Go to **AWS Console → Lambda**

![XRAY_4](/images/5-Workshop/3.api-gateway/3.7/xray_3.png)

- Select **Functions → DynamoDB_API_Handler**
- Go to  
  **Configuration → Monitoring and operations tools → Additional monitoring tools → Edit**

![XRAY_5](/images/5-Workshop/3.api-gateway/3.7/xray_4.png)

- Under **Lambda service traces**, enable **Enable**

![XRAY_6](/images/5-Workshop/3.api-gateway/3.7/xray_5.png)

---

#### **3. Access API Gateway**
- Go to **AWS Console → API Gateway**

![XRAY_7](/images/5-Workshop/3.api-gateway/3.7/xray_12.png)

- Select **APIs → FlyoraAPI**
- Go to  
  **Stages → Logs and tracing → Edit**

![XRAY_8](/images/5-Workshop/3.api-gateway/3.7/xray_6.png)

- Check **X-Ray tracing**

![XRAY_9](/images/5-Workshop/3.api-gateway/3.7/xray_7.png)

---

#### **4. Testing**
- Go to **AWS Console → Lambda**

![XRAY_10](/images/5-Workshop/3.api-gateway/3.7/xray_8.png)

- In the **Test** tab, click **Create new event**
- Event name: **test**
- Paste the JSON below into the event:

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






- Next, go to **AWS Console → X-ray**
![XRAY\_11](/images/5-Workshop/3.api-gateway/3.7/xray_9.png) 
- In tab **Traces**, a new trace ID will appear
![XRAY\_12](/images/5-Workshop/3.api-gateway/3.7/xray_10.png) 
- Click it to view detailed trace information




![XRAY\_13](/images/5-Workshop/3.api-gateway/3.7/xray_11.png) 