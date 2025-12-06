---
title : "Distribute with CloudFront "
weight: 2
chapter: false
pre: " <b> 5.4.2. </b> "
---

## 
### Create a CloudFront Distribution pointing to the S3 bucket 
* Go to the **Cloudfront** service.
![cloudfront_1](/images/5-Workshop/5.frontend/cloudfront_1.png)
* Create **Cloudfront Distribution**
![cloudfront_2](/images/5-Workshop/5.frontend/cloudfront_2.png)
*  Enter a  **Cloudfront name** (for example: `flyora-shop`).
![cloudfront_3](/images/5-Workshop/5.frontend/cloudfront_3.png)
* Select your **S3** you have created before
![cloudfront_4](/images/5-Workshop/5.frontend/cloudfront_4.png)
* Turn on Website endpoint make sure you get a url like this
```http://Your-S3-name.s3-website.ap-southeast-1.amazonaws.com/```
![cloudfront_5](/images/5-Workshop/5.frontend/cloudfront_5.png)
- Configure:
  - **Viewer protocol policy:** Redirect HTTP to HTTPS
  - **Allowed HTTP method:** GET, HEAD , OPTION
  - **Cache policy:** CatchingOptimized 
  - **Response headers policy:** CORS-with-preflight-and-SecurityHeadersPolicy
![cloudfront_6](/images/5-Workshop/5.frontend/cloudfront_6.png)
* Choose do not enable securiy protections
![cloudfront_7](/images/5-Workshop/5.frontend/cloudfront_7.png)
* Review and click **Create Distribution**
![cloudfront_8](/images/5-Workshop/5.frontend/cloudfront_8.png)
* After Create Distribution you have to wait for 5 or 10 minutes to deploy and if it deploy successfully it will show you date you have deploy 
![cloudfront_9](/images/5-Workshop/5.frontend/cloudfront_9.png)
