---
title : "Giới thiệu"
weight : 1
chapter : false
pre : " <b> 5.1. </b> "
---

## Giới thiệu Workshop

### Flyora là gì?

Flyora là nền tảng thương mại điện tử hiện đại được thiết kế để minh họa kiến trúc cloud-native sử dụng các dịch vụ serverless của AWS. Workshop này hướng dẫn bạn xây dựng một cửa hàng trực tuyến hoàn chỉnh với tính năng duyệt sản phẩm, chatbot hỗ trợ AI, và quản lý dữ liệu tự động.

### Mục tiêu Workshop

Sau khi hoàn thành workshop, bạn sẽ:

- Triển khai **hệ thống thương mại điện tử serverless** trên AWS
- Xây dựng **pipeline dữ liệu tự động** sử dụng S3 và Lambda triggers
- Phát triển **RESTful APIs** với API Gateway và Lambda
- Tạo **cơ sở dữ liệu có khả năng mở rộng** bằng DynamoDB
- Tích hợp **chatbot AI** cho hỗ trợ khách hàng
- Triển khai **frontend tĩnh** với S3 và CloudFront
- Thiết lập **CI/CD pipeline** cho triển khai tự động

### Tổng quan Kiến trúc

Nền tảng Flyora sử dụng kiến trúc serverless hoàn toàn:

**Tầng Frontend:**
- Website tĩnh được host trên **Amazon S3**
- Phân phối nội dung toàn cầu qua **Amazon CloudFront**
- Giao diện responsive cho duyệt và mua sắm sản phẩm

**Tầng Backend:**
- **API Gateway** cho các RESTful API endpoints
- **AWS Lambda** functions xử lý business logic
- **Amazon DynamoDB** lưu trữ dữ liệu sản phẩm và đơn hàng
- **Amazon S3** cho import và lưu trữ dữ liệu

**Tầng AI:**
- Chatbot hỗ trợ AI cho gợi ý sản phẩm
- Tích hợp vào giao diện frontend
- Hỗ trợ khách hàng theo thời gian thực

**Bảo mật & Xác thực:**
- **IAM roles** cho truy cập dịch vụ an toàn

### Cấu trúc Workshop

Workshop được tổ chức theo các module theo nhóm:

1. **Backend Team** - Phát triển API và data pipeline
2. **AI Team** - Tích hợp chatbot và tính năng AI
3. **Frontend Team** - Phát triển và triển khai UI
4. **CI/CD** - Pipeline triển khai tự động
5. **Testing** - Kiểm thử hệ thống và hiệu năng
6. **Cleanup** - Quản lý tài nguyên và tối ưu chi phí

### Kết quả Mong đợi

Sau khi hoàn thành workshop, bạn sẽ có:

- Website thương mại điện tử hoạt động hoàn chỉnh trên AWS
- Kinh nghiệm thực tế với kiến trúc serverless
- Hiểu biết về AWS best practices cho khả năng mở rộng và bảo mật
- Kiến thức triển khai CI/CD cho ứng dụng cloud
- Dự án portfolio thể hiện kỹ năng cloud engineering

### Chi phí

{{% notice info %}}
Workshop này được thiết kế để chạy trong **AWS Free Tier**. Tất cả dịch vụ sử dụng đều có tùy chọn free tier, và kiến trúc tránh các tài nguyên tốn kém như EC2 instances. Chi phí ước tính: **$0-5 USD** nếu hoàn thành trong vài giờ.
{{% /notice %}}

### Yêu cầu Trước khi Bắt đầu

Trước khi bắt đầu, đảm bảo bạn có:

- Tài khoản AWS với quyền quản trị
- Hiểu biết cơ bản về cloud computing
- Quen thuộc với REST APIs và JSON
- Kiến thức cơ bản về HTML/CSS/JavaScript (cho frontend)
- Git đã cài đặt trên máy local
