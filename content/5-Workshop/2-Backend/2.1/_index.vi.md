---
title : "Chuẩn bị & Cấu hình Lambda Trigger cho S3"
weight: 1
chapter: false
pre: " <b> 5.2.1. </b> "
---

## Chuẩn bị & Cấu hình Lambda Trigger cho S3

### Các bước thực hiện

#### 1. Truy cập dịch vụ IAM

* Vào **AWS Management Console** → tìm **IAM**.

![IAM\_1](/images/5-Workshop/2.prerequisite/iam_1.png)

* Chọn **Roles** → **Create Role**.

![IAM\_2](/images/5-Workshop/2.prerequisite/iam_2.png)

* Chọn **Trusted entity type:** *AWS service*.
* Chọn **Use case:** *Lambda*, sau đó nhấn **Next**.

![IAM\_3](/images/5-Workshop/2.prerequisite/iam_3.png)

#### 2. Gán quyền truy cập cho Role

* Gắn các policy sau:

  * `AmazonS3FullAccess`
  * `AmazonDynamoDBFullAccess_v2`
* Nhấn **Next**, đặt tên role là `LambdaS3DynamoDBRole`.

![IAM\_4](/images/5-Workshop/2.prerequisite/iam_4.png)
![IAM\_5](/images/5-Workshop/2.prerequisite/iam_5.png)

{{% notice note %}}
Role này giúp Lambda có thể đọc file từ **S3** và ghi dữ liệu vào **DynamoDB**.
{{% /notice %}}

#### Tạo S3 Bucket

* Truy cập dịch vụ **S3**.

![S3\_1](/images/5-Workshop/2.prerequisite/S3_1.png)

* Trong giao diện **S3**, chọn **Create bucket**.

![S3\_2](/images/5-Workshop/2.prerequisite/S3_2.png)

* Trong màn hình **Create bucket**:

  * **Bucket name:** Nhập tên, ví dụ:

    ```text
    flyora-bucket-database
    ```
  * (Nếu tên đã tồn tại, hãy thêm số phía sau.)

{{% notice note %}}
Giữ nguyên các thiết lập mặc định còn lại.
{{% /notice %}}

![S3\_2](/images/5-Workshop/2.prerequisite/S3_3.png)

* Xem lại cấu hình và chọn **Create bucket** để hoàn tất.

---

### Kết quả mong đợi

- Bucket **`flyora-bucket`** (hoặc tên bạn đã đặt) được tạo thành công.
- Role **`LambdaS3DynamoDBRole`** sẵn sàng để gán cho Lambda trong bước kế tiếp.

#### Cấu hình Lambda Trigger cho S3

Trong bước này, bạn sẽ cấu hình **AWS Lambda** để tự động import file CSV vào **DynamoDB** mỗi khi có file mới trong **S3 Bucket**.


1. **Tạo Lambda Function**
   - Truy cập **Lambda** → **Create function**.

![lambda_1](/images/5-Workshop/2.prerequisite/lambda_1.png)

   - Chọn **Author from scratch**.
   - Đặt tên: `AutoImportCSVtoDynamoDB`.
   - Runtime: **Python 3.13**.
   - Role: chọn **LambdaS3DynamoDBRole** đã tạo ở bước trước.

![lambda_2](/images/5-Workshop/2.prerequisite/lambda_2.png)

2. **Thêm Trigger**
   - Trong tab **Configuration → Triggers**, nhấn **Add trigger**.

![lambda_3](/images/5-Workshop/2.prerequisite/lambda_3.png)

   - Chọn **S3**.
   - Chọn Bucket `flyora-bucket`.
   - Event type: `All object create events`.

![lambda_4](/images/5-Workshop/2.prerequisite/lambda_4.png)

   - Nhấn **Add** để lưu.

![lambda_5](/images/5-Workshop/2.prerequisite/lambda_5.png)

1. **Dán code Lambda**
   - Dán đoạn code dưới đây:
    ```python
    import boto3
    import csv
    import io
    import json
    from botocore.exceptions import ClientError
    from decimal import Decimal

    dynamodb = boto3.resource('dynamodb')
    s3 = boto3.client('s3')

    # -------------------------
    # Hàm kiểm tra kiểu dữ liệu của mẫu (Detect Type)
    # -------------------------
    def detect_type(value):
        val_str = str(value).strip()
        # Check Int/Float
        try:
            float(val_str)
            return 'N' # Number
        except ValueError:
            pass
        return 'S' # String

    # -------------------------
    # Hàm chuyển đổi dữ liệu (Convert)
    # -------------------------
    def convert_value(value):
        if value is None: return None
        val_str = str(value).strip()
        if val_str == "": return None

        # Int check
        try:
            if float(val_str).is_integer():
                return int(float(val_str))
        except ValueError:
            pass

        # Decimal check (cho Float)
        try:
            return Decimal(val_str)
        except Exception:
            pass

        # Boolean
        if val_str.lower() == "true": return True
        if val_str.lower() == "false": return False

        return val_str

    # -------------------------
    # Tạo bảng Dynamic dựa trên kiểu dữ liệu phát hiện được
    # -------------------------
    def create_table_if_not_exists(table_name, pk_name, pk_type):
        existing_tables = dynamodb.meta.client.list_tables()['TableNames']
        if table_name in existing_tables:
            print(f"Table '{table_name}' already exists.")
            return

        print(f"Creating table: {table_name} | PK: {pk_name} | Type: {pk_type}")

        table = dynamodb.create_table(
            TableName=table_name,
            KeySchema=[{'AttributeName': pk_name, 'KeyType': 'HASH'}],
            AttributeDefinitions=[{'AttributeName': pk_name, 'AttributeType': pk_type}],
            BillingMode='PAY_PER_REQUEST'
        )
        table.wait_until_exists()
        print("Table created successfully.")

    # -------------------------
    # Main Handler
    # -------------------------
    def lambda_handler(event, context):
        try:
            for record in event['Records']:
                bucket = record['s3']['bucket']['name']
                key = record['s3']['object']['key']

                print(f"Processing: {key}")
                response = s3.get_object(Bucket=bucket, Key=key)
                
                # 1. QUAN TRỌNG: Dùng 'utf-8-sig' để xóa BOM, giúp nhận diện số chính xác
                body = response['Body'].read().decode('utf-8-sig')

                reader = csv.DictReader(io.StringIO(body))
                # Clean headers
                reader.fieldnames = [name.strip() for name in reader.fieldnames]
                
                items = list(reader)
                if not items: continue

                # Lấy thông tin Partition Key (PK)
                pk_name = reader.fieldnames[0]
                table_name = key.split('.')[0]

                # 2. QUAN TRỌNG: Phát hiện kiểu dữ liệu dựa trên dòng đầu tiên
                first_pk_val = items[0].get(pk_name)
                pk_type = detect_type(first_pk_val) # Sẽ trả về 'N' nếu là số, 'S' nếu là chữ

                # Tạo bảng đúng kiểu (N hoặc S)
                create_table_if_not_exists(table_name, pk_name, pk_type)

                table = dynamodb.Table(table_name)
                
                count = 0
                with table.batch_writer() as batch:
                    for row in items:
                        clean_item = {}
                        is_valid = True

                        for k, v in row.items():
                            if not k or k.strip() == "": continue
                            
                            clean_k = k.strip()
                            val = convert_value(v) # Chuyển đổi sang Int/Decimal/Bool

                            if val is None: continue

                            # 3. QUAN TRỌNG: Xử lý Partition Key theo đúng kiểu của Bảng
                            if clean_k == pk_name:
                                if pk_type == 'N':
                                    # Nếu bảng là Number, bắt buộc Key phải là Number
                                    if not isinstance(val, (int, Decimal)):
                                        print(f"SKIPPING ROW: Key '{val}' is not a number but table requires Number.")
                                        is_valid = False
                                        break
                                else:
                                    # Nếu bảng là String, ép kiểu sang String
                                    val = str(val)
                            
                            clean_item[clean_k] = val

                        if is_valid and pk_name in clean_item:
                            batch.put_item(Item=clean_item)
                            count += 1

                print(f"Success: Imported {count} items into {table_name} (PK Type: {pk_type})")

            return {'statusCode': 200, 'body': json.dumps('OK')}

        except Exception as e:
            print(f"ERROR: {str(e)}")
            import traceback
            traceback.print_exc()
            return {'statusCode': 500, 'body': json.dumps(str(e))}


![lambda_6](/images/5-Workshop/2.prerequisite/lambda_6.png)

  - Nhấn **Deploy** và xác nhận **Successfully**.

![lambda_7](/images/5-Workshop/2.prerequisite/lambda_7.png)