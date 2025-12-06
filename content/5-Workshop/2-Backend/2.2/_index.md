---
title : "Verify Data in DynamoDB"
weight: 2
chapter: false
pre: " <b> 5.2.2. </b> "
---

## Verify Data in DynamoDB

In this step, you will verify that the data from the CSV file has been successfully imported into DynamoDB.

#### Steps to Perform

1. **Upload the CSV File to the Bucket**

{{% notice info %}}
Download the sample CSV file from [here](/files/database01.zip).
{{% /notice %}}

* In the newly created Bucket:

  * Go to the **Objects** tab → click **Upload**.
  * Extract file zip.
  * Drag and drop the **files**, then click **Upload**.

![test_1](/images/5-Workshop/2.prerequisite/test_1.png)
![test_2](/images/5-Workshop/2.prerequisite/test_2.png)
![test_3](/images/5-Workshop/2.prerequisite/test_3.png)

> [!TIP]
> After uploading, please wait about 3–5 minutes for the Lambda function to import the data.

1. **Access the DynamoDB Service**
   - Go to **AWS Management Console** → search for **DynamoDB**.
   - Select **Tables** → click on the `products` table.

![test_4](/images/5-Workshop/2.prerequisite/test_4.png)

3. **View the Data**
   - Open the **Explore items** tab.

![test_5](/images/5-Workshop/2.prerequisite/test_5.png)

   - Check the list of products that have been imported.

![test_6](/images/5-Workshop/2.prerequisite/test_6.png)

{{% notice info %}}
If you don't see any data, please check the following:
- The DynamoDB table name must match the CSV file name.  
- The CSV file must have a valid header.  
- The Lambda function must have sufficient permissions to access both S3 and DynamoDB.
{{% /notice %}}
