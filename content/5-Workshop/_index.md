---
title : "Workshop"
weight: 5
chapter: false
pre: " <b> 5. </b> "
---

# Deploying the Flyora E-commerce System on AWS

### Overview

In this workshop, you will deploy the core components of the **Flyora** platform using a **Serverless** architecture on AWS.  
The objective is to build a system that is scalable, cost-efficient, and easy to maintain.

Components to be deployed:

- **Frontend**: Store & deliver UI via **S3 + CloudFront**
- **Backend API**: Handle business logic with **API Gateway + AWS Lambda**
- **Database**: Manage product / order data using **DynamoDB + S3**
- **User Authentication**: Implemented via **Amazon Cognito**
- **Chatbot**: Product consultation assistant integrated into UI (handled by AI Team)

The workshop is divided into group roles for parallel development:
**Backend (BE), AI (Chatbot), and Frontend (FE)**.

---

### System Architecture

![System Architecture Diagram](https://phucqb.sirv.com/Images/4a27331b2b7fa421fd6e.jpg)

---

### Workshop Content

1. [Introduction: Objectives & Expected Outcomes](1-Introduction/)

2. **Backend Workshop (BE)** — Build API + Automated Data Import Pipeline
   - [Prepare & upload CSV data to S3](2-Backend/2.1/)
   - [Create Lambda to automatically write CSV data to DynamoDB (S3 Trigger)](2-Backend/2.2/)
   - [Create API Gateway and integrate Lambda as Backend API](2-Backend/2.3/)
   - [Test API via Postman / API Gateway Console](2-Backend/2.4/)

3. **AI Workshop (Chatbot)** — Product Consultation Support
   - [(Create VPC & Configure Security Groups for RDS and Lambda)](3-AI/3.1/)
   - [(Configure RDS and connect to Dbeaver)](3-AI/3.2/)
   - [(Creating logic for lambda function)](3-AI/3.3/)

4. **Frontend Workshop (FE)** — Display Data & Hosting website
   - [Hosting website with S3](4-Frontend/4.1/)
   -  [Distribute with CloudFront ](4-Frontend/4.2/)
   -  [API Integration](4-Frontend/4.3/)

5. [Set Up CI/CD for Automatic Deployment](5-CICD/)

6. [Resource Cleanup to Avoid Unnecessary Charges](6-Cleanup/)

---

{{% notice info %}}
This workshop is designed to run within the **AWS Free Tier**,  
using no EC2, no SSH, and no paid services beyond free tier limits.
{{% /notice %}}
