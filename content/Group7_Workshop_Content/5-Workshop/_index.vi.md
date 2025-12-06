---
title : "Workshop"
weight: 5
chapter: false
pre: " <b> 5. </b> "
---

# Triển khai hệ thống thương mại điện tử Flyora trên AWS

### Tổng quan

Trong workshop này, bạn sẽ triển khai các thành phần cốt lõi của nền tảng **Flyora** theo mô hình **Serverless** trên AWS.  
Mục tiêu là xây dựng hệ thống có khả năng mở rộng, tối ưu chi phí và dễ bảo trì.

Các thành phần triển khai:

- **Frontend**: Lưu trữ & phân phối giao diện qua **S3 + CloudFront**
- **Backend API**: Xử lý logic nghiệp vụ qua **API Gateway + AWS Lambda**
- **Cơ sở dữ liệu**: Quản lý dữ liệu sản phẩm / đơn hàng bằng **DynamoDB + S3**
- **Xác thực người dùng**: Thực hiện thông qua **Amazon Cognito**
- **Chatbot**: Hỗ trợ tư vấn sản phẩm, tích hợp vào UI (Nhóm AI thực hiện)

Workshop được phân chia theo vai trò nhóm để dễ triển khai song song:
**Backend (BE), AI (Chatbot), và Frontend (FE)**.

---

### Kiến trúc tổng thể

![Flyora Architecture](/images/5-Workshop/flyora-architecture.png)

---

### Nội dung Workshop

1. [Giới thiệu mục tiêu & kết quả kỳ vọng](1-Introduction/)

2. **Backend Workshop (BE)** — Xây dựng API + Pipeline import dữ liệu tự động
   - [Chuẩn bị & Cấu hình Lambda Trigger cho S3](2-Backend/2.1/)
   - [Tạo Lambda tự động ghi dữ liệu CSV vào DynamoDB (S3 Trigger)](2-Backend/2.2/)
   - [Tạo API Gateway và tích hợp Lambda làm Backend API](2-Backend/2.3/)
   - [Kiểm thử API bằng Postman / API Gateway Console](2-Backend/2.4/)

3. **AI Workshop (Chatbot)** — Hỗ trợ tư vấn sản phẩm
   - [Tạo VPC  & Cấu hình Sercurity Group cho RDS và Lambda](3-AI/3.1/)
   - [Cấu hình RDS và kết nối với Dbeaver](3-AI/3.2/)
   - [Tạo logic cho hàm lambda](3-AI/3.3/)

4. **Frontend Workshop (FE)** — Hiển thị dữ liệu & Hosting website
   -  [Hosting website với S3](4-Frontend/4.1/)
   -  [Phân phối với CloudFront](4-Frontend/4.2/)
   -  [Kết nối với API](4-Frontend/4.3/)

5. [Thiết lập CI/CD tự động deploy](5-CICD/)

6. [Dọn dẹp tài nguyên để tránh phát sinh chi phí](6-Cleanup/)

---

{{% notice info %}}
Workshop này được thiết kế chạy trong phạm vi **AWS Free Tier**,  
không sử dụng EC2, không SSH, và không yêu cầu dịch vụ trả phí.
{{% /notice %}}
