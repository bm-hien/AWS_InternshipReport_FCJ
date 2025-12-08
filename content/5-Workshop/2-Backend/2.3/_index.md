---
title : "Create Lambda Function to Handle DynamoDB Requests"
weight: 3
chapter: false
pre: " <b> 5.2.3. </b> "
---

### Objective
Create a new **Lambda Function** to process requests to DynamoDB through API Gateway.

---

## Steps

{{% notice info %}}
Download the backend file from [here](/files/backend12.zip).
{{% /notice %}}

---

### Step 1: Create IAM Role

1. Open **IAM Console → Roles → Create role**

![IAM_1](/images/5-Workshop/3.api-gateway/3.1/iam_1.png)

![IAM_2](/images/5-Workshop/3.api-gateway/3.1/iam_2.png)

2. Select  
   **Trusted entity: AWS Service → Lambda**

![IAM_3](/images/5-Workshop/3.api-gateway/3.1/iam_3.png)

3. Attach the following permissions:
   - `AmazonDynamoDBFullAccess`
   - `CloudWatchLogsFullAccess`

4. Set the role name: **LambdaAPIAccessRole**

![IAM_4](/images/5-Workshop/3.api-gateway/3.1/iam_4.png)
![IAM_5](/images/5-Workshop/3.api-gateway/3.1/iam_5.png)

---

### Step 2: Create Lambda Function

1. Go to **AWS Lambda → Create function**
2. Select **Author from scratch**
3. Function name: `DynamoDB_API_Handler`
4. Runtime: **Java 17**
5. Choose IAM Role: **LambdaAPIAccessRole**

![LAMBDA_4](/images/5-Workshop/3.api-gateway/3.1/lambda_4.png)

---

### Step 3: Deploy the JAR File

1. Go to **S3 → Upload → Add files**  
   Upload the jar file, then copy the **Object URL**

![LAMBDA_5](/images/5-Workshop/3.api-gateway/3.1/lambda_5.png)

2. Go to **AWS Lambda → Upload from S3**  
   Paste the object URL you copied

![LAMBDA_6](/images/5-Workshop/3.api-gateway/3.1/lambda_6.png)

3. Go to **AWS Lambda → Code → Runtime settings → Edit**
4. Set Handler:  
   `org.example.flyora_backend.handler.StreamLambdaHandler::handleRequest`

![LAMBDA_7](/images/5-Workshop/3.api-gateway/3.1/lambda_7.png)

---

### Step 4: Configure Lambda Function

1. Go to **AWS Lambda → Configuration → General configuration → Edit**
2. Set **Timeout: 1 min**

![LAMBDA_8](/images/5-Workshop/3.api-gateway/3.1/lambda_8.png)

3. Go to **AWS Lambda → Configuration → Environment variables → Edit**
4. Add:
   - **Key:** `APP_JWT_SECRET`  
   - **Value:** `huntrotflyorateam!@ky5group5member`

![LAMBDA_9](/images/5-Workshop/3.api-gateway/3.1/lambda_9.png)

5. Key: `GHN_TOKEN`; Value: `445c659d-5586-11f0-8c19-5aba781b9b65`