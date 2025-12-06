---
title : "Create VPC & Configure Security Groups for RDS and Lambda"
weight: 1
chapter: false
pre: " <b> 5.3.1. </b> "
---

## Create VPC & Configure Security Groups for RDS and Lambda

### Implementation Steps

#### 1. Access the VPC service

Go to **AWS Management Console** → search for **VPC**.

![VPC\_1](/images/5-Workshop/2.prerequisite/vpc_1.png)

* Select **Your VPCs** → **Create VPC**.

![VPC\_2](/images/5-Workshop/2.prerequisite/vpc_2.png)

* Select **VPC and more** and name the project.

![VPC\_3](/images/5-Workshop/2.prerequisite/vpc_3.png)

* In the VPC configuration section: for **Number of Availability Zones** select 1, for **Number of public subnets** select 1, for **Number of private subnets** select 2, and for **NAT gateways** select **Zonal**.

![VPC\_4](/images/5-Workshop/2.prerequisite/vpc_4.png)

**NAT gateways** choose **In 1 AZ**, **VPC endpoints** choose **None** → Create VPC.

![VPC\_5](/images/5-Workshop/2.prerequisite/vpc_5.png)

{{% notice note %}}
This process will take a few minutes to create the **NAT gateway**.
{{% /notice %}}

* After creation is complete, we can view how 'VPC and more' created the resources via the **Resource map**.

![VPC\_6](/images/5-Workshop/2.prerequisite/vpc_6.png)

* We will create an additional private subnet located in a different AZ to be able to create the RDS instance.

![VPC\_7](/images/5-Workshop/2.prerequisite/vpc_7.png)

#### 2. Set up Security Groups

* We will create 2 separate Security Groups (SG) for maximum security.

* Go to **Security Groups**, click **Create security group**. We will create 2 security groups for **Lambda** and **RDS**.

![SG\_1](/images/5-Workshop/2.prerequisite/SG_1.png)

* Name the security group and select the **VPC** we created in step 1, then click create.

![SG\_2](/images/5-Workshop/2.prerequisite/SG_2.png)

* Continue creating the SG for **RDS**, select the **VPC** from step 1. In the inbound rules section, for type select **PostgreSQL**, and for Source select the SG created for **Lambda**.

![SG\_3](/images/5-Workshop/2.prerequisite/SG_3.png)