---
title : "Dọn dẹp tài nguyên để tránh phát sinh chi phí"
weight: 6
chapter: false
pre: " <b> 5.6. </b> "
---

## Dọn dẹp tài nguyên AWS

Để tránh phát sinh chi phí không mong muốn, bạn cần xóa các tài nguyên AWS đã tạo trong workshop theo thứ tự sau:

---

### 1. Xóa EventBridge Rule

1. Vào **AWS Console → EventBridge**
2. Chọn **Rules**
3. Chọn rule **DatabaseBackup**
4. Chọn **Delete**
5. Xác nhận xóa

---

### 2. Xóa API Gateway

1. Vào **AWS Console → API Gateway**
2. Chọn **FlyoraAPI**
3. Chọn **Actions → Delete API**
4. Xác nhận xóa bằng cách nhập tên API
5. Nhấn **Delete**

---

### 3. Xóa Lambda Functions

1. Vào **AWS Console → Lambda**
2. Xóa các Lambda functions sau:
   - **DynamoDB_API_Handler**
   - **AutoImportCSVtoDynamoDB**
   - **DatabaseBackupFunction**
3. Với mỗi function:
   - Chọn function → **Actions → Delete**
   - Xác nhận xóa

---

### 4. Xóa DynamoDB Tables

1. Vào **AWS Console → DynamoDB**
2. Chọn **Tables**
3. Xóa tất cả các bảng đã tạo từ CSV:
   - Chọn tất cả bảng → **Delete**
   - Xác nhận xóa bằng cách nhập `delete`


---

### 5. Xóa S3 Buckets và Objects

#### 5.1. Xóa Bucket Database

1. Vào **AWS Console → S3**
2. Chọn bucket **flyora-bucket-database**
3. Xóa tất cả objects trong bucket:
   - Chọn **Empty bucket**
   - Xác nhận bằng cách nhập `permanently delete`
4. Sau khi bucket rỗng:
   - Chọn bucket → **Delete bucket**
   - Xác nhận bằng cách nhập tên bucket

#### 5.2. Xóa Bucket Backup

1. Chọn bucket **flyora-bucket-backup**
2. Xóa tất cả objects:
   - Chọn **Empty bucket**
   - Xác nhận xóa
3. Xóa bucket:
   - Chọn **Delete bucket**
   - Xác nhận bằng cách nhập tên bucket

---

### 6. Xóa IAM User và Access Key

1. Vào **AWS Console → IAM → Users**
2. Chọn user **test**
3. Vào tab **Security credentials**
4. Xóa **Access Key** đã tạo:
   - Chọn Access Key → **Actions → Delete**
5. Quay lại danh sách Users
6. Chọn user **test** → **Delete user**
7. Xác nhận xóa

---

### 7. Xóa IAM Roles

#### 7.1. Xóa LambdaAPIAccessRole

1. Vào **AWS Console → IAM → Roles**
2. Chọn **LambdaAPIAccessRole**
3. Gỡ bỏ các policies đã gắn:
   - `AmazonDynamoDBFullAccess`
   - `CloudWatchLogsFullAccess`
   - `AWSXRayDaemonWriteAccess`
4. Chọn **Delete role**
5. Xác nhận xóa

#### 7.2. Xóa LambdaS3DynamoDBRole

1. Chọn **LambdaS3DynamoDBRole**
2. Gỡ bỏ các policies:
   - `AmazonS3FullAccess`
   - `AmazonDynamoDBFullAccess_v2`
3. Chọn **Delete role**
4. Xác nhận xóa

#### 7.3. Xóa LambdaDynamoDBBackupRole

1. Chọn **LambdaDynamoDBBackupRole**
2. Gỡ bỏ các policies:
   - `AmazonDynamoDBReadOnlyAccess`
   - `AmazonS3FullAccess`
   - `AWSLambdaBasicExecutionRole`
3. Chọn **Delete role**
4. Xác nhận xóa

---

### 8. Xóa CloudWatch Logs

1. Vào **AWS Console → CloudWatch**
2. Chọn **Logs → Log groups**
3. Tìm và xóa các log groups liên quan:
   - `/aws/lambda/DynamoDB_API_Handler`
   - `/aws/lambda/AutoImportCSVtoDynamoDB`
   - `/aws/lambda/DatabaseBackupFunction`
   - `/aws/apigateway/FlyoraAPI`
4. Chọn log group → **Actions → Delete log group(s)**
5. Xác nhận xóa

---

### 9. Xóa X-Ray Traces (Tùy chọn)

{{% notice info %}}
X-Ray traces sẽ tự động hết hạn sau 30 ngày và không tính phí lưu trữ, nhưng bạn có thể xóa thủ công nếu muốn.
{{% /notice %}}

1. Vào **AWS Console → X-Ray**
2. Chọn **Traces**
3. Traces sẽ tự động bị xóa sau thời gian lưu trữ mặc định

### 10. Xóa RDS và Subnet groups

1. Vào **subnet group** chọn subnet group đã tạo và ấn **delete**
2. Vào **database** ấn vào **database** đã tạo → **Action** → **Delete**

### 11. Xóa Lambda BirdShopChatBot và layer
1. Vào **function** chọn **BirdShopChatBot** → **Action** → **Delete**
2. Vào **Layers** chọn layer đã tạo ấn **delete**

### 12. Xóa VPC và NAT gateway và Elastic IP, EC2
1. Vào **VPC** chọn NAT gateway đã tạo → **Action** → **Delete NAT gateway**
2. Chọn **Elastic IP** → **Action** → **Release Elastic IP addresses**
3. Sau khi xóa xong NAT gateway và Elastic IP qua phần **Your VPCs** ấn VPC đã tạo → **Action** → **Delete VPC**
4. Qua phần **EC2** chọn **Instances** **chọn EC2** đã tạo → **Instances state** → **Terminate instances**
---
### 13. Xóa Cloudfront
1. Vào **CloudFront**, chọn phân phối đã tạo → **Actions** → **Disable**. Chờ cho đến khi trạng thái chuyển sang Disabled.
2. Select the checkbox for the disabled distribution again.
3. Choose Delete and confirm the deletion. The distribution cannot be recovered once deleted.
### Kiểm tra lại

Sau khi hoàn tất các bước trên, hãy kiểm tra lại các dịch vụ sau để đảm bảo không còn tài nguyên nào:

- ✅ **EventBridge**: Không còn rule nào
- ✅ **API Gateway**: Không còn API nào
- ✅ **Lambda**: Không còn function nào (3 functions)
- ✅ **DynamoDB**: Không còn bảng nào
- ✅ **S3**: Không còn bucket nào (2 buckets)
- ✅ **Cloudfront**: Không còn cấu hình phân phối nội dung
- ✅ **IAM Users**: Không còn user test
- ✅ **IAM Roles**: Không còn 3 roles đã tạo
- ✅ **CloudWatch Logs**: Không còn log groups liên quan
- ✅ **X-Ray**: Traces sẽ tự động hết hạn
- ✅ **RDS**: Xóa thành công
- ✅ **NAT** gateway: Không còn nữa
- ✅ **Elastic IP**: Không còn nữa
- ✅ **EC2** : Đã được terminate
{{% notice warning %}}
Hãy chắc chắn rằng bạn đã xóa tất cả các tài nguyên để tránh phát sinh chi phí không mong muốn. Đặc biệt chú ý xóa S3 buckets vì chúng có thể tích lũy dữ liệu theo thời gian.
{{% /notice %}}
