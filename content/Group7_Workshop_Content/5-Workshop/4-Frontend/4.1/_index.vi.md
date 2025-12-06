---
title : "Hosting website với S3"
weight: 1
chapter: false
pre: " <b> 5.4.1. </b> "
---

## 
### Bước 1: Tạo một S3 Bucket
* Truy cập vào dịch vụ **S3**.
![S3_hosting_website1](/images/5-Workshop/5.frontend/S3_hosting_website1.png)

* Nhấn **Create bucket**.
![S3_hosting_website2](/images/5-Workshop/5.frontend/S3_hosting_website2.png)
* Nhập một **Tên Bucket** duy nhất (ví dụ: `flyora-shop`).
* **Bỏ chọn** “Block all public access” 
![S3_hosting_website3](/images/5-Workshop/5.frontend/S3_hosting_website3.png)
* Xác nhận cảnh báo về quyền truy cập công khai.
![S3_hosting_website4](/images/5-Workshop/5.frontend/S3_hosting_website4.png)
* Nhấn **Create bucket**.
![S3_hosting_website5](/images/5-Workshop/5.frontend/S3_hosting_website5.png)

### Bước 2: Tải lên các tệp website
* Mở bucket vừa tạo.
![S3_hosting_website5](/images/5-Workshop/5.frontend/S3_hosting_website6.png)
* Nhấn **Upload** → **Add files** → chọn các tệp website của bạn (ví dụ: `index.html`)
![S3_hosting_website6](/images/5-Workshop/5.frontend/S3_hosting_website7.png)
* Nhấn **Upload**.
![S3_hosting_website7](/images/5-Workshop/5.frontend/S3_hosting_website8.png)

### Bước 3: Bật tính năng Static Website Hosting
* Chuyển đến tab **Properties** của bucket.
![S3_hosting_website8](/images/5-Workshop/5.frontend/S3_hosting_website9.png)
* Kéo xuống **Static website hosting**.
![S3_hosting_website9](/images/5-Workshop/5.frontend/S3_hosting_website10.png)
* Nhấn **Edit** → Bật **Static website hosting**
* Nhập:
   - **Index document:** `index.html`
   - **Error document:** `index.html` (tùy chọn)
![S3_hosting_website11](/images/5-Workshop/5.frontend/S3_hosting_website11.png)
* Nhấn **Save changes**.
![S3_hosting_website12](/images/5-Workshop/5.frontend/S3_hosting_website12.png)
* Chuyển sang tab **Permission** của bucket.
![S3_hosting_website13](/images/5-Workshop/5.frontend/S3_hosting_website13.png)
* Chỉnh sửa **Bucket Policy**
* Dán JSON policy sau (thay `flyora-shop` bằng tên bucket của bạn):
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicReadGetObject",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::your-bucket-name/*"
    }
  ]
}
```
### Bước 4: Kiểm tra Website
* Nhấn vào Bucket website endpoint URL
 ```http://your-bucket-name.s3-website-ap-southeast-1.amazonaws.com```
 Đảm bảo website hiển thị như hình dưới:
 ![S3_hosting_website14](/images/5-Workshop/5.frontend/S3_hosting_website14.png)
