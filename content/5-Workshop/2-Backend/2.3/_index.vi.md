---
title : "Tạo API Gateway và tích hợp Lambda"
weight: 3
chapter: false
pre: " <b> 5.2.3. </b> "
---
### Mục tiêu
Tạo **Lambda Function** mới để xử lý các yêu cầu đến DynamoDB thông qua API Gateway.




## Các bước thực hiện
{{% notice info %}}
Tải file backend từ [đây](/files/backend12.zip).
{{% /notice %}}
### Bước 1: Tạo IAM Role
1. Mở **IAM Console → Roles → Create role**  


![IAM\_1](/images/5-Workshop/3.api-gateway/3.1/iam_1.png)




![IAM\_2](/images/5-Workshop/3.api-gateway/3.1/iam_2.png)


1. Chọn **Trusted entity: AWS Service → Lambda**  


![IAM\_3](/images/5-Workshop/3.api-gateway/3.1/iam_3.png)


2. Gán quyền:
   - `AmazonDynamoDBFullAccess`
   - `CloudWatchLogsFullAccess`
3. Đặt tên role: `LambdaAPIAccessRole`


![IAM\_4](/images/5-Workshop/3.api-gateway/3.1/iam_4.png)
![IAM\_5](/images/5-Workshop/3.api-gateway/3.1/iam_5.png)
---


### Bước 2: Tạo Lambda Function
1. Vào **AWS Lambda → Create function**
2. Chọn **Author from scratch**
3. Nhập tên: `DynamoDB_API_Handler`
4. Runtime: **Java 17**
5. Chọn IAM Role: `LambdaAPIAccessRole`


![LAMBDA\_4](/images/5-Workshop/3.api-gateway/3.1/lambda_4.png)
---


### Bước 3: Deploy file jar
1. Vào **S3 → Upload → Add files**: Chọn file jar. Sau đó copy object Url
![LAMBDA\_5](/images/5-Workshop/3.api-gateway/3.1/lambda_5.png)
2. Vào **AWS Lambda → Upload from**: dan s3 url vua tai len
![LAMBDA\_6](/images/5-Workshop/3.api-gateway/3.1/lambda_6.png)
3. Vào **AWS Lambda → Code → Runtime settings -> Edit**
4. Handler: `org.example.flyora_backend.handler.StreamLambdaHandler::handleRequest`
![LAMBDA\_7](/images/5-Workshop/3.api-gateway/3.1/lambda_7.png)
5. Vào **AWS Lambda → Code -> Configuration → Edit**:
6. Timeout: 1 min
![LAMBDA\_8](/images/5-Workshop/3.api-gateway/3.1/lambda_8.png) 
3. Vào **AWS Lambda → Configuration → Environment variables → Edit** 
4. Key: `APP_JWT_SECRET`; Value: `huntrotflyorateam!@ky5group5member` 
![LAMBDA\_9](/images/5-Workshop/3.api-gateway/3.1/lambda_9.png)

5. Tương tự:
    Key: `GHN_TOKEN`; Value: `445c659d-5586-11f0-8c19-5aba781b9b65`



