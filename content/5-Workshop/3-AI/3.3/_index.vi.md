---
title : "Tạo logic cho hàm lambda"
weight: 3
chapter: false
pre: " <b> 5.3.3. </b> "
---

## Tạo logic cho hàm lambda

### Các bước thực hiện

#### 1. Truy cập dịch vụ lambda

* Vào **AWS Management Console** → tìm **Lambda**.

![lambda_chatbot\_1](/images/5-Workshop/2.prerequisite/lambda_chatbot_1.png)

* Đầu tiên qua phần layer tạo một layer để hàm **lambda** có thư viện kết nối với **PostgreSQL** (psycopg2).

* Truy cập vào link github để có thể tải phiên bản python phù hợp ở đây là sử dụng psycopg2-3.11 https://github.com/jkehler/awslambda-psycopg2

* Sau khi tải xong bỏ tất cả folder psycopg2-3.11 vào một folder đặt tên là (`python`) sau đó zip folder đó lại và đặt tên  (`postgres-layer-3.11`)

![lambda_chatbot\_2](/images/5-Workshop/2.prerequisite/lambda_chatbot_2.png)

* Bấm create layer đặt tên cho layer xong tải file zip postgres-layer-3.11 lên → create

![lambda_chatbot\_3](/images/5-Workshop/2.prerequisite/lambda_chatbot_3.png)

#### 2. Tạo Lambda

* Functions → Create function

![lambda_chatbot\_4](/images/5-Workshop/2.prerequisite/lambda_chatbot_4.png)

* Đặt tên cho hàm lambda và chọn runtime Python 3.11 

![lambda_chatbot\_5](/images/5-Workshop/2.prerequisite/lambda_chatbot_5.png)

* Additional configurations tích chọn VPC chọn VPC đã được tạo Subnet chọn 1 private subnet Security group chọn Lambda-SG 

![lambda_chatbot\_6](/images/5-Workshop/2.prerequisite/lambda_chatbot_6.1.png)

* Sau khi hàm lambda được tạo chúng ta sẽ đến phần layer add layer đã được tạo 

![lambda_chatbot\_7](/images/5-Workshop/2.prerequisite/lambda_chatbot_7.png)

* Custom layer chọn layer đã tạo version chọn 1 → Add

![lambda_chatbot\_8](/images/5-Workshop/2.prerequisite/lambda_chatbot_8.png)

* Qua phần Configuration ở phần General configuration chỉnh thời gian lên 1p

![lambda_chatbot\_9](/images/5-Workshop/2.prerequisite/lambda_chatbot_9.png)

* Permissions thêm AmazonBedrockFullAccess và AWSLambdaVPCAccessExecutionRole

![lambda_chatbot\_10](/images/5-Workshop/2.prerequisite/lambda_chatbot_10.png)

* Environment variables thêm các biến: DB_HOST, DB_NAME, DB_USER, DB_PASS (Điền thông tin RDS của bạn vào).

![lambda_chatbot\_11](/images/5-Workshop/2.prerequisite/lambda_chatbot_11.png)

* Sau khi cấu hình xong dán dòng code python này vô hàm lambda và nhấn nút deploy
    ```python
    import json
    import boto3
    import psycopg2
    import os


    # --- CẤU HÌNH ---
    DB_HOST = os.environ.get('DB_HOST')
    DB_NAME = os.environ.get('DB_NAME')
    DB_USER = os.environ.get('DB_USER')
    DB_PASS = os.environ.get('DB_PASS')


    # Dùng vùng US East 1 cho ổn định
    bedrock = boto3.client(service_name='bedrock-runtime', region_name='ap-southeast-1')


    # --- 1. HÀM EMBEDDING (COHERE) ---
    def get_embedding(text):
        try:
            body = json.dumps({
                "texts": [text],
                "input_type": "search_query",
                "truncate": "END"
            })
            response = bedrock.invoke_model(
                body=body,
                modelId="cohere.embed-multilingual-v3",
                accept="application/json", contentType="application/json"
            )
            return json.loads(response['body'].read())['embeddings'][0]
        except Exception as e:
            print(f"Lỗi Embed: {e}")
            return None


    # --- 2. HÀM XỬ LÝ METADATA AN TOÀN ---
    def format_product_info(metadata):
        """
        Hàm này giúp chuẩn hóa dữ liệu dù file csv thiếu cột
        """
        if not metadata: return None
    
        # Lấy tên (Ưu tiên các key phổ biến)
        name = metadata.get('name') or metadata.get('product_name') or metadata.get('title') or "Sản phẩm không tên"
    
        # Lấy giá (Nếu không có thì để rỗng hoặc 'Liên hệ')
        price = metadata.get('price') or metadata.get('gia') or metadata.get('display_price')
        price_str = f"- Giá: {price}" if price else ""
    
        # Lấy loại (nếu có từ script import thông minh)
        category = metadata.get('type') or "Sản phẩm"
    
        # Tạo chuỗi mô tả cho AI đọc
        # Ví dụ: "Loài chim: Vẹt. - Giá: 500k"
        ai_context = f"Danh mục/Sản phẩm: {name} ({category}) {price_str}"
    
        # Tạo object cho Frontend hiển thị (Product Card)
        frontend_card = {
            "id": metadata.get('id') or metadata.get('product_id'),
            "name": name,
            "price": price if price else "Liên hệ", # Frontend sẽ hiện chữ "Liên hệ" nếu không có giá
            "image": metadata.get('image_url') or metadata.get('link_anh') or "", # Ảnh có thể rỗng
            "type": category # Để frontend biết đây là chim hay đồ chơi
        }
    
        return ai_context, frontend_card


    # --- 3. MAIN HANDLER ---
    def lambda_handler(event, context):
        print("Event:", event)
    
        try:
            # Parse Input
            if 'body' in event:
                try:
                    body_data = json.loads(event['body']) if isinstance(event['body'], str) else event['body']
                except: body_data = {}
            else: body_data = event
            
            user_question = body_data.get('question', '')
            if not user_question: return {'statusCode': 400, 'body': json.dumps('Thiếu câu hỏi')}


            # A. Tạo Vector
            q_vector = get_embedding(user_question)
            if not q_vector: return {'statusCode': 500, 'body': json.dumps('Lỗi tạo vector')}


            # B. Tìm kiếm DB
            conn = psycopg2.connect(host=DB_HOST, database=DB_NAME, user=DB_USER, password=DB_PASS)
            cur = conn.cursor()
        
            # Lấy metadata lên để xử lý
            sql = """
                SELECT content, metadata
                FROM knowledge_base
                ORDER BY embedding <=> %s
                LIMIT 3
            """
            cur.execute(sql, (json.dumps(q_vector),))
            results = cur.fetchall()
            cur.close(); conn.close()


            # C. Xử lý kết quả (QUAN TRỌNG NHẤT)
            ai_contexts = []
            frontend_products = []


            for row in results:
                raw_content = row[0] # Nội dung gốc lúc import
                raw_metadata = row[1] # JSON metadata
            
                # Gọi hàm chuẩn hóa
                ai_text, card_data = format_product_info(raw_metadata)
            
                if ai_text:
                    # Gộp nội dung gốc + nội dung định danh lại cho chắc ăn
                    ai_contexts.append(f"{ai_text}. Chi tiết: {raw_content}")
                    frontend_products.append(card_data)


            # Nếu không tìm thấy gì
            if not ai_contexts:
                final_answer = "Xin lỗi, hiện tại shop chưa tìm thấy thông tin phù hợp trong kho dữ liệu ạ."
            else:
                # D. Gửi cho LLM (Claude 3 Haiku)
                context_str = "\n---\n".join(ai_contexts)
            
                system_prompt = (
                    "Bạn là nhân viên tư vấn của Shop Chim, tên là 'Vẹt Tinh Thông'. "
                    "Phong cách: Thân thiện, Lịch sự nhưng Ngắn gọn.\n\n"
                
                    "HƯỚNG DẪN XỬ LÝ (ƯU TIÊN THEO THỨ TỰ):\n"
                    "1. GIAO TIẾP XÃ GIAO: Nếu khách chỉ chào hỏi (Xin chào, Hi...) hoặc cảm ơn: "
                    "-> Hãy chào lại thân mật và hỏi khách cần tìm gì. TUYỆT ĐỐI KHÔNG tự ý liệt kê sản phẩm nếu khách chưa hỏi.\n"
                    "   (Lưu ý: Đừng nhầm từ 'Chào' trong 'Xin chào' với chim 'Chào mào'. Nếu khách chỉ chào, hãy lờ đi dữ liệu về chim Chào mào).\n\n"
                
                    "2. TƯ VẤN SẢN PHẨM: Khi khách hỏi về hàng hóa:\n"
                    "-> Trả lời súc tích: Tên sản phẩm + Giá + Tình trạng kho (nếu có).\n"
                    "-> Không mô tả dài dòng văn hoa trừ khi khách hỏi chi tiết 'như thế nào', 'ra sao'.\n"
                    "-> Nếu không có thông tin trong Context, hãy nói 'Dạ shop chưa có món này ạ'.\n"
                )
            
                user_msg = f"Thông tin tham khảo:\n{context_str}\n\nCâu hỏi: {user_question}"
            
                # Gọi Claude 3 Haiku
                claude_body = json.dumps({
                    "anthropic_version": "bedrock-2023-05-31",
                    "max_tokens": 300,
                    "temperature": 0.1,
                    "system": system_prompt,
                    "messages": [{"role": "user", "content": user_msg}]
                })


                try:
                    model_res = bedrock.invoke_model(
                        body=claude_body,
                        modelId="anthropic.claude-3-haiku-20240307-v1:0"
                    )
                    res_json = json.loads(model_res['body'].read())
                    final_answer = res_json['content'][0]['text']
                except Exception as e:
                    print(f"Lỗi gọi LLM: {e}")
                    final_answer = "Xin lỗi, hệ thống AI đang bận, vui lòng thử lại sau."


            # E. Trả về kết quả
            return {
                'statusCode': 200,
                'headers': {
                    'Access-Control-Allow-Origin': '*',
                    'Access-Control-Allow-Methods': 'POST',
                    'Content-Type': 'application/json'
                },
                'body': json.dumps({
                    "answer": final_answer,
                    "products": frontend_products # Frontend dùng cái này để vẽ UI
                })
            }


        except Exception as e:
            print(f"Lỗi System: {e}")
            return {'statusCode': 500, 'body': json.dumps(str(e))}

![lambda_chatbot\_12](/images/5-Workshop/2.prerequisite/lambda_chatbot_12.png)


#### 3. Tích hợp vào API Gateway của nhóm

* Truy cập dịch vụ API Gateway

![api\_1](/images/5-Workshop/2.prerequisite/api_1.png)

* Chọn API mà Backend đã tạo

![api\_2](/images/5-Workshop/2.prerequisite/api_2.png)

* Chọn Create resource

![api\_3](/images/5-Workshop/2.prerequisite/api_3.png)

* Resource name `chatbot` và tích chọn vào CORS

![api\_4](/images/5-Workshop/2.prerequisite/api_4.png)

* Chọn vào chatbot và tích chọn vào Create method

![api\_5](/images/5-Workshop/2.prerequisite/api_5.png)

* Method type chọn POST tích chọn vào Lambda proxy integration

![api\_6](/images/5-Workshop/2.prerequisite/api_6.png)

* Chọn **VPC** đã tạo xong ấn Create method

![api\_7](/images/5-Workshop/2.prerequisite/api_7.png)

* Sau khi cấu hình xong bấm deploy API

![api\_8](/images/5-Workshop/2.prerequisite/api_8.png)

