---
title : "Testing API using Postman"
weight: 5
chapter: false
pre: " <b> 5.2.5. </b> "
---

### Objective
Test the API Gateway REST endpoint integrated with the Lambda Function to verify the data operations performed on DynamoDB.

---

{{% notice note %}}
Download and install [Postman](https://dl.pstmn.io/download/latest/win64) before starting this section.
{{% /notice %}}

---

## 1. **Update Authorization Settings**
- Go to **AWS Console → API Gateway**
- Select **FlyoraAPI**
- Navigate to  
  **/api/v1/{myProxy+} → ANY → Method request → Edit**

![POST_5](/images/5-Workshop/3.api-gateway/3.3/post_5.png)

- Set Authorization to **AWS_IAM**(Note: Only enable when testing with Postman, then remember to turn it off afterwards)

![POST_6](/images/5-Workshop/3.api-gateway/3.3/post_6.png)

---

## 2. **Create an Access Key**
- Go to **AWS Console → IAM → Users**
- Click **Create User**

![POST_8](/images/5-Workshop/3.api-gateway/3.3/post_8.png)

- Set username: **test**

![POST_7](/images/5-Workshop/3.api-gateway/3.3/post_7.png)

- Confirm user creation

![POST_9](/images/5-Workshop/3.api-gateway/3.3/post_9.png)

- Open the **test** user → **Security credentials → Create access key**

![POST_10](/images/5-Workshop/3.api-gateway/3.3/post_10.png)

- Choose **Local code**

![POST_11](/images/5-Workshop/3.api-gateway/3.3/post_11.png)

- Copy the **Access key** and **Secret access key**

![POST_12](/images/5-Workshop/3.api-gateway/3.3/post_12.png)

---

## **Testing GET Request**
- Open **Postman**
- Choose **GET**
- Enter URL:```https://<api_id>.execute-api.ap-southeast-1.amazonaws.com/dev/api/v1/reviews/product/1```

- **Headers** tab:  
  - Key: `Content-Type` | Value: `application/json`

- **Authorization** tab:  
  - Type: **AWS Signature**  
  - Enter **AccessKey**  
  - Enter **SecretKey**  
  - AWS Region: `ap-southeast-1`  
  - Service Name: `execute-api`

- Click **Send**

- Result: Returns the list of `Items` from the **reviews** table.

![POST_13](/images/5-Workshop/3.api-gateway/3.3/post_13.png)

---

## **Testing POST Request**
- Choose **POST**
- URL:```https://<api_id>.execute-api.ap-southeast-1.amazonaws.com/dev/api/v1/reviews/submit```

- **Body → raw → JSON**
  ```json
  {
    "customerId": 2,
    "rating": 4,
    "comment": "Chim ăn ngon và vui vẻ!",
    "customerName": "Nguyễn Văn B"
  }
- Click Send

- Result: Inserts a new Item into the Review table.




