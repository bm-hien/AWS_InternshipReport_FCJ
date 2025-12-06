---
title : "Cấu hình RDS và kết nối với Dbeaver"
weight: 2
chapter: false
pre: " <b> 5.3.2. </b> "
---

## Cấu hình RDS và kết nối với Dbeaver

### Các bước thực hiện

#### 1. Truy cập dịch vụ RDS

* Vào **AWS Management Console** → tìm **RDS**.

![rds\_1](/images/5-Workshop/2.prerequisite/rds_1.png)

* Trước khi tạo RDS chúng ta sẽ tạo subnet group

![rds\_2](/images/5-Workshop/2.prerequisite/rds_2.png)

* Đặt tên cho subnet group và chọn **VPC** đã tạo 
  
![rds\_3](/images/5-Workshop/2.prerequisite/rds_3.png)

* AZ chọn ap-southeast-1a và ap-southeast-1b Subnet chọn 2 private subnet sau đó ấn Create 

![rds\_4](/images/5-Workshop/2.prerequisite/rds_4.png)

#### 2. Tạo RDS

* Vào database → Create database

![rds\_5](/images/5-Workshop/2.prerequisite/rds_5.png)

* Ở phần cấu hình database chọn Full configuration chọn **PostgreSQL** ờ phần Templates chọn Sandbox ở phần Settings đặt tên cho DB và đặt mật khẩu 

![rds\_6](/images/5-Workshop/2.prerequisite/rds_6.png)

* Cấu hình các phần còn lại như sau

![rds\_7](/images/5-Workshop/2.prerequisite/rds_7.png)

* Ở phần Connectivity chọn **VPC** đã tạo và DB subnet group và Public access chọn **No** và phần VPC security group chọn Security group đã tạo cho RDS còn lại giữ nguyên và bấm Create

![rds\_8](/images/5-Workshop/2.prerequisite/rds_8.png)

#### 1. Lưu trữ dữ liệu và kết nối PostgreSQL bằng DBeaver

* Bạn có thể tải **Dbeaver** tại đây: https://dbeaver.io/

{{% notice info %}}
Tải file knowledge_base từ [đây](/files/knowledge_base.zip).
{{% /notice %}}

* Để có thể kết nối từ máy local tới **Dbeaver** chúng ta cần tạo một **EC2** để có thể làm cầu nối

* Truy cập vào **EC2** → Launch instance

![ec2\_1](/images/5-Workshop/2.prerequisite/ec2_1.png)

* Đặt tên cho **EC2** và chọn Instance type t2.micro tạo một key pair và lưu trong máy

![ec2\_2](/images/5-Workshop/2.prerequisite/ec2_2.png)

* Ở phần Network settings chọn **VPC** đã tạo chọn public subnet và tạo một Security group 

![ec2\_3](/images/5-Workshop/2.prerequisite/ec2_3.png)

* Inbound Security Group Rules chọn my ip xong Launch instance 

![ec2\_4](/images/5-Workshop/2.prerequisite/ec2_4.png)

* Tiếp theo chúng ta sẽ qua phần Security group của **RDS** chỉnh sửa phần inbound rules chúng ta sẽ add rule mới  type: **PostgreSQL** và Source là **EC2** vừa tạo

![ec2\_5](/images/5-Workshop/2.prerequisite/ec2_5.png)

* Truy cập vào dbeaver ấn vào phần connection

![ec2\_6](/images/5-Workshop/2.prerequisite/ec2_6.png)

* Chọn PostgreSQL ở phần Host copy port Endpoint của RDS và các thông tin bạn đã tạo RDS từ đầu 

![ec2\_7](/images/5-Workshop/2.prerequisite/ec2_7.png)

* Ấn vào dấu + SSH  ở phần Host/IP copy public IP của **EC2** User Name điền `ec2-user` ở phần Authentication Method chọn Public Key và chọn key pair đã tạo lúc tạo **EC2** sau đó ấn Test connection rồi Finish

![ec2\_8](/images/5-Workshop/2.prerequisite/ec2_8.png)

* Sau khi kết nối thành công mở sql script và dán dòng code này để tạo bảng knowledge_base sau khi tạo bảng refresh lại database để hiện bảng knowledge_base

    ```sql
        -- 1. Bật extension vector (chỉ chạy 1 lần)
        CREATE EXTENSION IF NOT EXISTS vector;

        -- 2. Tạo bảng kiến thức (Knowledge Base)
        CREATE TABLE knowledge_base (
        id bigserial PRIMARY KEY,
        content text,             -- Nội dung text gốc (đoạn văn đã chia nhỏ)
        metadata jsonb,           -- Lưu thêm info: link ảnh, tên file, page number...
        embedding vector(1024)    -- QUAN TRỌNG: Phải là 1024 cho Cohere Multilingual
        );

        -- 3. Tạo index để tìm kiếm nhanh hơn
        CREATE INDEX ON knowledge_base USING hnsw (embedding vector_cosine_ops)
        WITH (m = 16, ef_construction = 64);


![ec2\_9](/images/5-Workshop/2.prerequisite/ec2_9.png)

* Để có thể import dữ liệu vào Dbeaver bằng python chúng ta cần ssh thông qua cmd chúng ta sẽ mở cmd ở nơi lưu trữ keypair và copy câu lệnh này 

    ```bash
    ssh -i "my-key.pem" -L 5433:RDS endpoint port:5432 ec2-user@public IP EC2 -N

![ec2\_10](/images/5-Workshop/2.prerequisite/ec2_10.png)

* Sau đó chúng ta chạy hàm python này để import dữ liệu vô Dbeaver

    ```python
    import pandas as pd
    import json
    import boto3
    import psycopg2
    import time
    import glob
    import os
    import numpy as np
    from dotenv import load_dotenv

    # ==========================================
    # 1. CẤU HÌNH & BẢO MẬT
    # ==========================================
    current_dir = os.path.dirname(os.path.abspath(__file__))
    env_path = os.path.join(current_dir, 'pass.env')
    load_dotenv(env_path)

    CSV_FOLDER = './database' 
    DB_HOST = os.getenv("DB_HOST")
    DB_NAME = os.getenv("DB_NAME")
    DB_USER = os.getenv("DB_USER")
    DB_PASS = os.getenv("DB_PASS")

    # Kết nối AWS
    bedrock = boto3.client(
        service_name='bedrock-runtime', 
        region_name='ap-southeast-1', 
        aws_access_key_id=os.getenv("aws_access_key_id"),
        aws_secret_access_key=os.getenv("aws_secret_access_key")
    )

    # ==========================================
    # 2. TỪ ĐIỂN DỊCH (QUAN TRỌNG NHẤT)
    # ==========================================

    # A. Dịch Tên Cột (Cho AI hiểu ngữ cảnh)
    COLUMN_MAP = {
        "price": "Giá bán", "gia": "Giá bán", "cost": "Chi phí", "fee": "Phí",
        "stock": "Tồn kho", "so_luong": "Tồn kho",
        "description": "Mô tả", "mo_ta": "Mô tả", "chi_tiet": "Chi tiết",
        "origin": "Xuất xứ", "xuat_xu": "Xuất xứ",
        "material": "Chất liệu", "chat_lieu": "Chất liệu",
        "color": "Màu sắc", "mau_sac": "Màu sắc",
        "weight": "Trọng lượng", "trong_luong": "Trọng lượng",
        "food_type": "Loại thức ăn",
        "usage_target": "Phù hợp cho loài chim",
        "furniture_type": "Loại nội thất",
        "time": "Thời gian xử lý", "thoi_gian": "Thời gian xử lý",
        "method_name": "Tên phương thức"
    }

    # B. Dịch Giá Trị (Cho Website hiển thị tiếng Việt) <--- PHẦN BẠN CẦN
    VALUE_TRANSLATIONS = {
        "FOODS": "Đồ ăn",
        "Foods": "Đồ ăn",
        "foods": "Đồ ăn",
        
        "TOYS": "Đồ chơi",
        "Toys": "Đồ chơi",
        "toys": "Đồ chơi",
        
        "FURNITURE": "Nội thất",
        "Furniture": "Nội thất",
        "furniture": "Nội thất",
        
        "Bird": "Chim cảnh",
        "bird": "Chim cảnh"
    }

    # --- CÁC HÀM PHỤ TRỢ ---
    def get_embedding(text):
        try:
            if not text or len(str(text)) < 5: return None
            body = json.dumps({"texts": [str(text)], "input_type": "search_document", "truncate": "END"})
            response = bedrock.invoke_model(body=body, modelId="cohere.embed-multilingual-v3", accept="application/json", contentType="application/json")
            return json.loads(response['body'].read())['embeddings'][0]
        except: return None

    def clean(val):
        if pd.isna(val) or str(val).lower() in ['nan', 'none', '', 'null']: return ""
        val_str = str(val).strip()
        
        # --- DỊCH TỰ ĐỘNG Ở ĐÂY ---
        # Nếu giá trị có trong từ điển dịch thì thay thế luôn
        if val_str in VALUE_TRANSLATIONS:
            return VALUE_TRANSLATIONS[val_str]
        
        return val_str

    def main():
        try:
            conn = psycopg2.connect(host=DB_HOST, database=DB_NAME, user=DB_USER, password=DB_PASS, port=5433) # Lưu ý port SSH 5433
            cur = conn.cursor()
            print("✅ Kết nối Database thành công!")
        except Exception as e:
            print(f"❌ Lỗi kết nối DB: {e}"); return

        csv_files = glob.glob(os.path.join(CSV_FOLDER, "*.csv"))
        print(f"📂 Tìm thấy {len(csv_files)} file CSV.")

        # Biến thống kê
        stats = {"bird": 0, "food": 0, "toy": 0, "furniture": 0, "best_sellers": []}
        total_success = 0

        for file_path in csv_files:
            filename = os.path.basename(file_path).lower()
            print(f"\n--- Đang xử lý file: {filename} ---")
            
            try:
                df = pd.read_csv(file_path)
                df = df.replace({np.nan: None}) 
                
                # Tự động nhận diện loại (Prefix)
                category_prefix = "Sản phẩm"
                if "bird" in filename: category_prefix = "Loài chim"
                elif "food" in filename: category_prefix = "Thức ăn chim"
                elif "toy" in filename or "do_choi" in filename: category_prefix = "Đồ chơi chim"
                elif "furniture" in filename: category_prefix = "Nội thất lồng chim"
                elif "ship" in filename or "delivery" in filename: category_prefix = "Phương thức vận chuyển"
                elif "payment" in filename: category_prefix = "Phương thức thanh toán"

                for index, row in df.iterrows():
                    # Thống kê
                    if "bird" in filename: stats["bird"] += 1
                    elif "food" in filename: stats["food"] += 1
                    elif "toy" in filename: stats["toy"] += 1
                    elif "furniture" in filename: stats["furniture"] += 1

                    # A. ĐỊNH DANH
                    p_id = clean(row.get('id') or row.get('product_id') or row.get('payment_id'))
                    name = clean(row.get('name') or row.get('product_name') or row.get('title') or row.get('method_name'))
                    if not name: 
                        if p_id: name = f"Mã {p_id}"
                        else: continue

                    # B. QUÉT CỘT TỰ ĐỘNG VÀ DỊCH
                    content_parts = [f"{category_prefix}: {name}"]
                    
                    # Quét toàn bộ cột, nếu có trong COLUMN_MAP thì thêm vào
                    for col_key, col_val in row.items():
                        val_clean = clean(col_val) # Hàm clean sẽ tự động dịch FOODS -> Đồ ăn
                        if val_clean and col_key in COLUMN_MAP:
                            content_parts.append(f"{COLUMN_MAP[col_key]}: {val_clean}")

                    # C. XỬ LÝ RIÊNG GIÁ & TỒN KHO & BÁN CHẠY
                    price = clean(row.get('price') or row.get('gia') or row.get('fee'))
                    if price: content_parts.append(f"Giá: {price}")
                    
                    stock = clean(row.get('stock') or row.get('so_luong'))
                    if stock: content_parts.append(f"Tồn kho: {stock}")

                    sold = clean(row.get('sold') or row.get('da_ban'))
                    if sold:
                        content_parts.append(f"Đã bán: {sold}")
                        try:
                            if float(sold) > 0: stats["best_sellers"].append((float(sold), name, category_prefix))
                        except: pass

                    content_to_embed = ". ".join(content_parts) + "."

                    # D. TẠO METADATA (Cũng dùng giá trị đã dịch)
                    # Lưu ý: Hàm clean() ở trên đã dịch rồi, nên ta gọi lại clean() cho từng field
                    metadata = {}
                    for k, v in row.items():
                        metadata[k] = clean(v) # Lưu vào metadata bản tiếng Việt luôn
                    
                    # Ghi đè các trường chuẩn
                    metadata['id'] = p_id
                    metadata['name'] = name
                    metadata['price'] = price if price else "Liên hệ"
                    metadata['type'] = category_prefix
                    metadata['image'] = clean(row.get('image_url') or row.get('link_anh'))
                    metadata['sold'] = sold

                    # E. INSERT
                    vector = get_embedding(content_to_embed)
                    if vector:
                        cur.execute(
                            "INSERT INTO knowledge_base (content, embedding, metadata) VALUES (%s, %s, %s)",
                            (content_to_embed, json.dumps(vector), json.dumps(metadata, default=str))
                        )
                        total_success += 1
                        if total_success % 10 == 0:
                            print(f"   -> Đã nạp {total_success} dòng...")
                            conn.commit()
                            time.sleep(0.1) 

            except Exception as e:
                print(f"⚠️ Lỗi xử lý file {filename}: {e}"); continue

        # --- TẠO BẢN TIN THỐNG KÊ ---
        print("\n--- Đang tạo bản tin thống kê... ---")
        top_products = sorted(stats["best_sellers"], key=lambda x: x[0], reverse=True)[:5]
        top_names = ", ".join([f"{p[1]} ({int(p[0])} lượt mua)" for p in top_products])
        
        summary_content = (
            f"BÁO CÁO THỐNG KÊ SHOP CHIM: "
            f"Tổng số chim: {stats['bird']}. Đồ ăn: {stats['food']}. "
            f"Đồ chơi: {stats['toy']}. Nội thất: {stats['furniture']}. "
            f"TOP 5 SẢN PHẨM BÁN CHẠY NHẤT: {top_names}."
        )
        
        summary_vector = get_embedding(summary_content)
        if summary_vector:
            cur.execute("INSERT INTO knowledge_base (content, embedding, metadata) VALUES (%s, %s, %s)",
                        (summary_content, json.dumps(summary_vector), json.dumps({"id":"STATS","name":"Thống kê"}, default=str)))
            conn.commit()

        cur.close(); conn.close()
        print(f"\n🎉 HOÀN TẤT! Tổng cộng đã import: {total_success + 1} dòng.")

    if __name__ == "__main__":
        main()

* Sau khi import xong refresh lại bảng knowledge_base để thấy kết quả

![ec2\_11](/images/5-Workshop/2.prerequisite/ec2_11.png)