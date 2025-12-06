---
title : "Hosting website with S3"
weight: 1
chapter: false
pre: " <b> 5.4.1. </b> "
---

## 
### Step 1: Create an S3 Bucket
* Go to the **S3** service.
![S3_hosting_website1](/images/5-Workshop/5.frontend/S3_hosting_website1.png)
* Click **Create bucket**.
![S3_hosting_website2](/images/5-Workshop/5.frontend/S3_hosting_website2.png)
* Enter a unique **Bucket name** (for example: `flyora-shop`).
* **Uncheck** “Block all public access” 
![S3_hosting_website3](/images/5-Workshop/5.frontend/S3_hosting_website3.png)
* Acknowledge the warning about public access.
![S3_hosting_website4](/images/5-Workshop/5.frontend/S3_hosting_website4.png)
* Click **Create bucket**.
* ![S3_hosting_website5](/images/5-Workshop/5.frontend/S3_hosting_website5.png)
### Step 2: Upload Website Files
* Open your newly created bucket.
![S3_hosting_website5](/images/5-Workshop/5.frontend/S3_hosting_website6.png)
* Click **Upload** → **Add files** → select your website files (e.g, `index.html`)
![S3_hosting_website6](/images/5-Workshop/5.frontend/S3_hosting_website7.png)
* * Click **Upload**.
![S3_hosting_website7](/images/5-Workshop/5.frontend/S3_hosting_website8.png)
### Step 3: Enable Static Website Hosting

* Go to the **Properties** tab of your bucket.
![S3_hosting_website8](/images/5-Workshop/5.frontend/S3_hosting_website9.png)
* Scroll down to **Static website hosting**.
![S3_hosting_website9](/images/5-Workshop/5.frontend/S3_hosting_website10.png)
* Click **Edit** → Enable **Static website hosting**
* Enter:
   - **Index document:** `index.html`
   - **Error document:** `index.html` (optional)
![S3_hosting_website11](/images/5-Workshop/5.frontend/S3_hosting_website11.png)
* Click **Save changes**.
![S3_hosting_website12](/images/5-Workshop/5.frontend/S3_hosting_website12.png)
*Go to the **Permission** tab of your bucket.
![S3_hosting_website13](/images/5-Workshop/5.frontend/S3_hosting_website13.png)
* Edit **Bucket Policy**
* Paste the following JSON policy (replace `flyora-shop` with your bucket name):
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
### Step 4: Test Your Website
Click the **Bucket website endpoint URL**.
```http://your-bucket-name.s3-website-ap-southeast-1.amazonaws.com```
* Make sure it looks like this
* ![S3_hosting_website14](/images/5-Workshop/5.frontend/S3_hosting_website14.png)

