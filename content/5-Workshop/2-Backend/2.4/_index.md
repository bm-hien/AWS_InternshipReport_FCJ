---
title : "Create API Gateway and Integrate with Lambda"
weight: 4
chapter: false
pre: " <b> 5.2.4. </b> "
---

### Objective
Connect **AWS API Gateway** with a **Lambda Function** to create a RESTful endpoint that allows accessing data stored in **DynamoDB**.

---

### Implementation Steps

---

#### **1. Access API Gateway**
- Go to **AWS Console → API Gateway**
- Click **Create API**

![API_1](/images/5-Workshop/3.api-gateway/3.2/api_1.png)

- Select **REST API (Build)**

![API_2](/images/5-Workshop/3.api-gateway/3.2/api_2.png)

- Configure:
  - **Create new API:** New API  
  - **API name:** `FlyoraAPI`  
  - **Endpoint type:** Regional  
- Click **Create API**

![API_3](/images/5-Workshop/3.api-gateway/3.2/api_3.png)

---

#### **2. Create Resources and Methods**

1. In the sidebar, select  
   **Actions → Create Resource**

![API_4](/images/5-Workshop/3.api-gateway/3.2/api_4.png)

- **Resource Name:** `api`  
- Click **Create Resource**

![API_5](/images/5-Workshop/3.api-gateway/3.2/api_10.png)

---

2. Select **/api → Actions → Create Resource**

---

3. Configure the resource:

- **Resource path:** /api/  
- **Resource Name:** `v1`  
- Click **Create Resource**

![API_14](/images/5-Workshop/3.api-gateway/3.2/api_14.png)

---

4. Create a proxy resource under `/api`

- Check **Proxy resource**
- **Resource path:** /api/  
- **Resource Name:** `{proxy+}`
- Click **Create Resource**

![API_18](/images/5-Workshop/3.api-gateway/3.2/api_18.png)

---

5. Select **/v1 → Actions → Create Resource**

Configure:

- Check **Proxy resource**
- **Resource path:** /api/v1/  
- **Resource Name:** `{myProxy+}`
- Click **Create Resource**

![API_15](/images/5-Workshop/3.api-gateway/3.2/api_15.png)

---

6. **Enable CORS** for all resources

![API_CORS](/images/5-Workshop/3.api-gateway/3.2/api_19.png)

Under **OPTIONS → Integration response → Header Mappings**, ensure the headers below exist:

- **Access-Control-Allow-Origin:** `*`
- **Access-Control-Allow-Headers:**  
  `Content-Type,X-Amz-Date,Authorization,X-Api-Key,X-Amz-Security-Token`
- **Access-Control-Allow-Methods:**  
  `DELETE,GET,HEAD,OPTIONS,PATCH,POST,PUT`

![API_Headers](/images/5-Workshop/3.api-gateway/3.2/api_20.png)

---

#### **3. Integrate with Lambda**

1. After creating **/api/v1/{myProxy+}**, the **ANY** method appears:
   - Select **ANY → Integration request → Edit**

![API_16](/images/5-Workshop/3.api-gateway/3.2/api_16.png)

---

2. Attach Lambda:
- **Integration type:** Lambda Function  
- Check **Lambda proxy integration**  
- **Lambda Region:** `ap-southeast-1` (Singapore)  
- **Lambda Function:** select your `Lambda_API_Handler`

![API_17](/images/5-Workshop/3.api-gateway/3.2/api_17.png)

---

#### **4. Deploy API**
- Select **Actions → Deploy API**
- **Deployment stage:** New stage  
  - **Stage name:** `dev`  
  - **Description:** Development stage for Lambda API  
- Click **Deploy**

![API_9](/images/5-Workshop/3.api-gateway/3.2/api_9.png)

---

After deployment, you will receive an Invoke URL in the format:


```https://<api_id>.execute-api.ap-southeast-1.amazonaws.com/dev```

