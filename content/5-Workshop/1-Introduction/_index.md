---
title : "Introduction"
weight: 1
chapter: false
pre: " <b> 5.1. </b> "
---

## Introduction

### What is Flyora?

Flyora is a modern e-commerce platform designed to demonstrate cloud-native architecture using AWS serverless services. This workshop guides you through building a fully functional online store with product browsing, AI-powered chatbot assistance, and automated data management.

### Workshop Objectives

By completing this workshop, you will:

- Deploy a **serverless e-commerce system** on AWS
- Implement **automated data pipelines** using S3 and Lambda triggers
- Build **RESTful APIs** with API Gateway and Lambda
- Create a **scalable database** using DynamoDB
- Integrate **AI chatbot** functionality for customer support
- Deploy a **static frontend** with S3 and CloudFront
- Set up **CI/CD pipelines** for automated deployments

### Architecture Overview

The Flyora platform uses a fully serverless architecture:

**Frontend Layer:**
- Static website hosted on **Amazon S3**
- Global content delivery via **Amazon CloudFront**
- Responsive UI for product browsing and shopping

**Backend Layer:**
- **API Gateway** for RESTful API endpoints
- **AWS Lambda** functions for business logic
- **Amazon DynamoDB** for product and order data
- **Amazon S3** for data import and storage

**AI Layer:**
- AI-powered chatbot for product recommendations
- Integrated into the frontend UI
- Provides real-time customer assistance

**Security & Authentication:**
- **Amazon Cognito** for user authentication
- **IAM roles** for secure service access

### Workshop Structure

This workshop is organized into team-based modules:

1. **Backend Team** - API development and data pipeline
2. **AI Team** - Chatbot integration and AI features
3. **Frontend Team** - UI development and deployment
4. **CI/CD** - Automated deployment pipeline
5. **Testing** - System validation and performance testing
6. **Cleanup** - Resource management and cost optimization

### Expected Outcomes

After completing this workshop, you will have:

- A fully functional e-commerce website running on AWS
- Hands-on experience with serverless architecture
- Understanding of AWS best practices for scalability and security
- Knowledge of CI/CD implementation for cloud applications
- A portfolio project demonstrating cloud engineering skills

### Cost Considerations

{{% notice info %}}
This workshop is designed to run within the **AWS Free Tier**. All services used have free tier options, and the architecture avoids costly resources like EC2 instances. Estimated cost for running this workshop: **$0-5 USD** if completed within a few hours.
{{% /notice %}}

### Prerequisites

Before starting, ensure you have:

- An AWS account with administrative access
- Basic understanding of cloud computing concepts
- Familiarity with REST APIs and JSON
- Basic knowledge of HTML/CSS/JavaScript (for frontend work)
- Git installed on your local machine
