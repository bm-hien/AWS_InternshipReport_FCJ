---
title : "AI Workshop (Chatbot)"
weight: 3
chapter: false
pre: " <b> 5.3. </b> "
---

Workshop này hướng dẫn chi tiết cách xây dựng một Chatbot tư vấn sản phẩm sử dụng kiến trúc RAG (Retrieval-Augmented Generation) trên nền tảng AWS.

## 1. Kiến trúc hệ thống

Hệ thống sử dụng các dịch vụ sau của AWS:
* **Amazon Bedrock:** Cung cấp các mô hình AI (LLM).
    * *Generation Model:* **Amazon Nova Lite** (`anthropic.claude-3-haiku-20240307-v1:0`) để trả lời câu hỏi.
    * *Embedding Model:* **Cohere Embed Multilingual** (`cohere.embed-multilingual-v3`) để vector hóa dữ liệu.
* **Amazon RDS (PostgreSQL):** Lưu trữ dữ liệu sản phẩm và vector (sử dụng extension `Dbeaver`).
* **AWS Lambda:** Hàm xử lý logic trung gian (Serverless backend).
