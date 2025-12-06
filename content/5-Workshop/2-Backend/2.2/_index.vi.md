---
title : "Kiểm tra dữ liệu trong DynamoDB"
weight: 2
chapter: false
pre: " <b> 5.2.2. </b> "
---

## Kiểm tra dữ liệu trong DynamoDB
Trong bước này, bạn sẽ xác minh rằng dữ liệu từ file CSV đã được import thành công vào DynamoDB.

#### Các bước thực hiện

1. **Upload dữ liệu CSV lên Bucket**

{{% notice info %}}
Tải file mẫu CSV từ [đây](/files/database02.zip).
{{% /notice %}}

* Trong Bucket vừa tạo:

  * Chọn tab **Objects** → **Upload**.
  * Giải nén file zip.
  * Kéo & thả các **file**, sau đó nhấn **Upload**.

![test_1](/images/5-Workshop/2.prerequisite/test_1.png)
![test_2](/images/5-Workshop/2.prerequisite/test_2.png)
![test_3](/images/5-Workshop/2.prerequisite/test_3.png)

> [!TIP]
> Sau khi upload xong hãy đợi khoảng 3-5 phút để lambda import dữ liệu

1. **Truy cập dịch vụ DynamoDB**
   - Vào **AWS Management Console** → tìm **DynamoDB**.
   - Chọn **Tables** → click vào bảng ví dụ: `Products`.

![test_4](/images/5-Workshop/2.prerequisite/test_4.png)

2. **Xem dữ liệu**
   - Chọn tab **Explore items**.

![test_5](/images/5-Workshop/2.prerequisite/test_5.png)

   - Kiểm tra danh sách sản phẩm đã được import.

![test_6](/images/5-Workshop/2.prerequisite/test_6.png)

{{% notice info %}}
Nếu không thấy dữ liệu, kiểm tra:
- Tên bảng DynamoDB phải trùng với tên file CSV.  
- File CSV phải có header hợp lệ.  
- Lambda có đủ quyền truy cập S3 và DynamoDB.
{{% /notice %}}

3. **Tạo GSI cho DynamoDB**

3. **Tạo GSI (Global Secondary Index) cho DynamoDB**

Để backend có thể truy vấn dữ liệu dựa trên các trường không phải khóa chính (ví dụ: tìm `Account` bằng `username`, tìm `Order` bằng `customer_id`), chúng ta cần tạo các **Global Secondary Indexes (GSI)** tương ứng với cấu trúc trong mã nguồn Java.

Chúng ta sẽ sử dụng **AWS CloudShell** để chạy script tự động tạo toàn bộ Index nhằm đảm bảo chính xác và tiết kiệm thời gian.

* Trên thanh điều hướng phía trên cùng (Top Navigation Bar), nhấp vào biểu tượng **CloudShell** (hình terminal `>_`).

![cloudshell01](/images/5-Workshop/3.api-gateway/3.2/cloudshell_01.png)

* Đợi vài giây để môi trường dòng lệnh khởi động.
* Tại dòng lệnh CloudShell, tạo một file script mới tên là `create_gsi.sh`:

* Xóa file (nếu đã tạo trước đó): `rm create_all_gsi.sh`
* Tạo [**File**](/files/ai_studio_code.sh) mới: `nano create_all_gsi.sh`

  - Tải file về mở lên rồi copy toàn bộ vào Cloudshell.
   
  - Ctrl + O (Để lưu) -> Nhấn Enter
  - Ctrl + X (Để thoát)
* Cấp quyền chạy cho file: `chmod +x create_all_gsi.sh`
* Khởi chạy file: `./create_all_gsi.sh`

{{% notice info %}}
Thời gian đợi sẽ tùy thuộc vào việc Indexs được tạo ra trong bao lâu
{{% /notice %}}