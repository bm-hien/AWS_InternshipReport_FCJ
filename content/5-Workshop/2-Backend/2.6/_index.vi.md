---
title : "S3 CSV Backup"
weight: 6
chapter: false
pre: " <b> 5.2.6. </b> "
---

## S3 CSV Backup

### Tạo IAM Role cho Lambda

* Vào **AWS Management Console** → tìm **IAM**.

![IAM\_1](/images/5-Workshop/2.prerequisite/iam_1.png)

* Chọn **Roles** → **Create Role**.

![IAM\_2](/images/5-Workshop/2.prerequisite/iam_2.png)

* Chọn **Trusted entity type:** *AWS service*.
* Chọn **Use case:** *Lambda*, sau đó nhấn **Next**.

![IAM\_3](/images/5-Workshop/2.prerequisite/iam_3.png)

#### Gán quyền truy cập cho Role

* Gắn các policy sau:

  * `AmazonDynamoDBReadOnlyAccess`
  * `AmazonS3FullAccess`
  * `AWSLambdaBasicExecutionRole`
* Nhấn **Next**, đặt tên role là `LambdaDynamoDBBackupRole`.

![backup_1](/images/5-Workshop/3.api-gateway/3.6/backup_1.png)

{{% notice note %}}
Role này cho phép Lambda quét toàn bộ bảng DynamoDB và lưu bản backup dưới dạng CSV vào S3 Bucket.
{{% /notice %}}

### Tạo S3 Bucket

* Truy cập dịch vụ **S3**.

![S3\_1](/images/5-Workshop/2.prerequisite/S3_1.png)

* Trong giao diện **S3**, chọn **Create bucket**.

![S3\_2](/images/5-Workshop/2.prerequisite/S3_2.png)

* Trong màn hình **Create bucket**:

  * **Bucket name:** Nhập tên, ví dụ:

    ```text
    flyora-bucket-backup
    ```
  * (Nếu tên đã tồn tại, hãy thêm số phía sau.)

{{% notice note %}}
Giữ nguyên các thiết lập mặc định còn lại.
{{% /notice %}}

* Xem lại cấu hình và chọn **Create bucket** để hoàn tất.

---

### Cấu hình Lambda Trigger cho S3
1. **Tạo Lambda Function**
   - Truy cập **Lambda** → **Create function**.

![lambda_1](/images/5-Workshop/2.prerequisite/lambda_1.png)

   - Chọn **Author from scratch**.
   - Đặt tên: `DatabaseBackupFunction`.
   - Runtime: **Python 3.14**.
   - Role: chọn **LambdaDynamoDBBackupRole** đã tạo ở bước trước.

2. **Vào Configuration → Environment variables**

![backup_2](/images/5-Workshop/3.api-gateway/3.6/backup_2.png)

  - Chọn **Edit**.
  - **Add environment variable**
  - **Key**: `BUCKET_NAME`
  - **Value**: `flyora-bucket-backup`
  - Chọn **Save**.

3. **Code**

    ```python
    import boto3
    import csv
    import io
    import os
    from datetime import datetime
    from boto3.dynamodb.conditions import Key

    s3 = boto3.client('s3')
    dynamodb = boto3.resource('dynamodb')

    def scan_all(table):
        """Scan toàn bộ bảng DynamoDB, xử lý paging."""
        items = []
        response = table.scan()
        items.extend(response.get('Items', []))

        while 'LastEvaluatedKey' in response:
            response = table.scan(ExclusiveStartKey=response['LastEvaluatedKey'])
            items.extend(response.get('Items', []))

        return items


    def lambda_handler(event, context):
        bucket = os.environ["BUCKET_NAME"]
        tables = [t.strip() for t in os.environ["TABLE_LIST"].split(",")]

        timestamp = datetime.utcnow().strftime("%Y-%m-%d-%H-%M-%S")

        for table_name in tables:
            try:
                table = dynamodb.Table(table_name)
                data = scan_all(table)

                if not data:
                    print(f"Table {table_name} EMPTY → skip")
                    continue

                # Lấy danh sách tất cả fields
                all_keys = sorted({key for item in data for key in item.keys()})

                # Convert to CSV
                csv_buffer = io.StringIO()
                writer = csv.DictWriter(csv_buffer, fieldnames=all_keys)
                writer.writeheader()
                for item in data:
                    writer.writerow({k: item.get(k, "") for k in all_keys})

                key = f"dynamo_backup/{table_name}/{table_name}_{timestamp}.csv"

                s3.put_object(
                    Bucket=bucket,
                    Key=key,
                    Body=csv_buffer.getvalue().encode("utf-8")
                )

                print(f"Backup xong bảng {table_name} → {key}")

            except Exception as e:
                print(f"Lỗi khi backup bảng {table_name}: {e}")

        return {
            "status": "completed",
            "tables": tables
        }
    ``` 
  - Chọn **Deploy**.

4. **Cấu hình chạy tự động (Schedule)**

- Lambda → Triggers → Add Trigger → EventBridge (Schedule)

- **Rule name**: `DatabaseBackup`
- **Rule description**: `AutoBackup in 4 days`
- **Rule type**: Schedule expression
- **Schedule expression**: `rate(4 days)`

![backup_2](/images/5-Workshop/3.api-gateway/3.6/backup_3.png)