---
title : "Phân phối với CloudFront"
weight: 2
chapter: false
pre: " <b> 5.4.2. </b> "
---

### Tạo CloudFront Distribution trỏ đến S3 bucket
* Truy cập dịch vụ **CloudFront**.
![cloudfront_1](/images/5-Workshop/5.frontend/cloudfront_1.png)
* Nhấn **Create CloudFront Distribution** (Tạo Phân phối CloudFront).
![cloudfront_2](/images/5-Workshop/5.frontend/cloudfront_2.png)
* Nhập một **Tên CloudFront** (ví dụ: `flyora-shop`).
![cloudfront_3](/images/5-Workshop/5.frontend/cloudfront_3.png)
* Chọn **S3 bucket** mà bạn đã tạo trước đó.
![cloudfront_4](/images/5-Workshop/5.frontend/cloudfront_4.png)
* Bật **Website endpoint** (Điểm cuối Website), đảm bảo bạn nhận được URL có dạng như sau:
```[http://Tên-S3-của-bạn.s3-website.ap-southeast-1.amazonaws.com/](http://Tên-S3-của-bạn.s3-website.ap-southeast-1.amazonaws.com/)```
![cloudfront_5](/images/5-Workshop/5.frontend/cloudfront_5.png)
- Cấu hình:
  - **Viewer protocol policy (Chính sách giao thức người xem):** Redirect HTTP to HTTPS (Chuyển hướng HTTP sang HTTPS)
  - **Allowed HTTP method (Phương thức HTTP được phép):** GET, HEAD, OPTION
  - **Cache policy (Chính sách cache):** CachingOptimized (Tối ưu hóa cache)
  - **Response headers policy (Chính sách tiêu đề phản hồi):** CORS-with-preflight-and-SecurityHeadersPolicy
![cloudfront_6](/images/5-Workshop/5.frontend/cloudfront_6.png)
* Chọn **do not enable security protections** (không bật tính năng bảo vệ bảo mật).
![cloudfront_7](/images/5-Workshop/5.frontend/cloudfront_7.png)
* **Review** (Xem lại) và nhấn **Create Distribution** (Tạo Phân phối).
![cloudfront_8](/images/5-Workshop/5.frontend/cloudfront_8.png)
* Sau khi tạo Distribution, bạn cần chờ **5 đến 10 phút** để quá trình triển khai (deploy) hoàn tất. Nếu triển khai thành công, nó sẽ hiển thị ngày bạn đã triển khai.
![cloudfront_9](/images/5-Workshop/5.frontend/cloudfront_9.png)
