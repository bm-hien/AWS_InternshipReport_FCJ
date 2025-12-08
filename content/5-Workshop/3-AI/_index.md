---
title : "AI Workshop (Chatbot)"
weight: 3
chapter: false
pre: " <b> 5.3. </b> "
---

This workshop provides a detailed guide on how to build a Product Consultation Chatbot using RAG (Retrieval-Augmented Generation) architecture on the AWS platform.

## 1. System Architecture

The system utilizes the following AWS services:
* **Amazon Bedrock:** Provides AI models (LLMs).
    * *Generation Model:* **Amazon Nova Lite** (`anthropic.claude-3-haiku-20240307-v1:0`) for answering questions.
    * *Embedding Model:* **Cohere Embed Multilingual** (`cohere.embed-multilingual-v3`) for data vectorization.
* **Amazon RDS (PostgreSQL):** Stores product data and vectors (using the `Dbeaver` extension).
* **AWS Lambda:** Intermediate logic processing function (Serverless backend).
