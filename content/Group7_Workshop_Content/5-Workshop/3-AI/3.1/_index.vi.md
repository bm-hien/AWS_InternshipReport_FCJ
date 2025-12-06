---
title : "Tạo VPC  & Cấu hình Sercurity Group cho RDS và Lambda"
weight: 1
chapter: false
pre: " <b> 5.3.1. </b> "
---

## Tạo VPC  & Cấu hình Sercurity Group cho RDS và Lambda

### Các bước thực hiện

#### 1. Truy cập dịch vụ VPC

Vào **AWS Management Console** → tìm **VPC**.

![VPC\_1](/images/5-Workshop/2.prerequisite/vpc_1.png)

* Chọn **Your VPCs** → **Create VPC**.

![VPC\_2](/images/5-Workshop/2.prerequisite/vpc_2.png)

* Chọn **VPC and more** và đặt tên cho project

![VPC\_3](/images/5-Workshop/2.prerequisite/vpc_3.png)

* Ở phần cấu hình cho VPC **Number of Availability Zones** chọn 1 **Number of public subnets** chọn 1 và **Number of private subnets** chọn 2 và **NAT gateways** chọn **Zonal**

![VPC\_4](/images/5-Workshop/2.prerequisite/vpc_4.png)

**NAT gateways** chọn **In 1 Az** **VPC endpoints** chọn **None** → Create VPC

![VPC\_5](/images/5-Workshop/2.prerequisite/vpc_5.png)

{{% notice note %}}
Quá trình này sẽ diễn ra vài phút để có thể tạo **NAT gateway**.
{{% /notice %}}

* Sau khi tạo xong chúng ta có thể coi cách Vpc and more tạo ra các tài nguyên thông qua **Resource map**

![VPC\_6](/images/5-Workshop/2.prerequisite/vpc_6.png)

* Chúng ta sẽ tạo thêm một private subnet đặt ở một AZ khác để có thể tạo RDS được

![VPC\_7](/images/5-Workshop/2.prerequisite/vpc_7.png)

#### 2. Thiết lập Security Groups

* Chúng ta sẽ tạo 2 Security Group (SG) riêng biệt để bảo mật tối đa.

* Vào phần **Security Groups** bấm **Create security group** chúng ta sẽ tạo 2 security group cho **Lambda** và **RDS**

![SG\_1](/images/5-Workshop/2.prerequisite/SG_1.png)

* Đặt tên cho security group và chọn **VPC** chúng ta đã tạo ở bước 1 sau đó bấm create

![SG\_2](/images/5-Workshop/2.prerequisite/SG_2.png)

* Tiếp tục tạo SG cho **RDS** chọn **VPC** ở bước 1 ở phần inbound rules ở phần type chọn **PostgreSQL** ở phần Source chọn SG đã tạo cho **Lambda**

![SG\_3](/images/5-Workshop/2.prerequisite/SG_3.png)