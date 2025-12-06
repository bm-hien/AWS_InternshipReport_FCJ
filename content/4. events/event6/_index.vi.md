---
title: "Event 6"
weight: 4.6
chapter: false
pre: " <b> 4.6. </b> "

---

# Event 6 :: AWS Well-Architected Security Pillar

## Chi tiết sự kiện
- **Ngày:** Thứ Sáu, 29/11/2025
- **Thời gian:** 8:30 AM – 12:00 PM (Chỉ buổi sáng)
- **Địa điểm:** AWS Vietnam Office

## 8:30 – 8:50 AM | Khai mạc & Nền tảng Security
- Vai trò Security Pillar trong Well-Architected
- Nguyên tắc cốt lõi: Least Privilege – Zero Trust – Defense in Depth
- Shared Responsibility Model
- Top threats trong môi trường cloud tại Việt Nam

## ⭐ Pillar 1 — Identity & Access Management
### 8:50 – 9:30 AM | Modern IAM Architecture
- IAM: Users, Roles, Policies – tránh long-term credentials
- IAM Identity Center: SSO, permission sets
- SCP & permission boundaries cho multi-account
- MFA, credential rotation, Access Analyzer
- **Mini Demo:** Validate IAM Policy + simulate access

## ⭐ Pillar 2 — Detection
### 9:30 – 9:55 AM | Detection & Continuous Monitoring
- CloudTrail (org-level), GuardDuty, Security Hub
- Logging tại mọi tầng: VPC Flow Logs, ALB/S3 logs
- Alerting & automation với EventBridge
- Detection-as-Code (infrastructure + rules)

## 9:55 – 10:10 AM | Coffee Break

## ⭐ Pillar 3 — Infrastructure Protection
### 10:10 – 10:40 AM | Network & Workload Security
- VPC segmentation, private vs public placement
- Security Groups vs NACLs: mô hình áp dụng
- WAF + Shield + Network Firewall
- Workload protection: EC2, ECS/EKS security basics

## ⭐ Pillar 4 — Data Protection
### 10:40 – 11:10 AM | Encryption, Keys & Secrets
- KMS: key policies, grants, rotation
- Encryption at-rest & in-transit: S3, EBS, RDS, DynamoDB
- Secrets Manager & Parameter Store — patterns rotation
- Data classification & access guardrails

## ⭐ Pillar 5 — Incident Response
### 11:10 – 11:40 AM | IR Playbook & Automation
- IR lifecycle theo AWS
- **Playbook:**
  - Compromised IAM key
  - S3 public exposure
  - EC2 malware detection
- Snapshot, isolation, evidence collection
- Auto-response bằng Lambda/Step Functions

## 11:40 – 12:00 PM | Wrap-Up & Q&A
- Tổng kết 5 pillars
- Common pitfalls & thực tế doanh nghiệp Việt Nam
- Roadmap security learning (Security Specialty, SA Pro)

## Hình ảnh sự kiện

![Workshop Security](/images/event6/FCJ.jpg)
*Workshop AWS Well-Architected Security Pillar*

![Security Groups](/images/event6/securitygroups.jpg)
*Cấu hình Security Groups*

![KMS Workflow](/images/event6/KMSworkflow.jpg)
*Quy trình mã hóa KMS*

![Incident Response](/images/event6/incidentResponse.jpg)
*Playbook Incident Response*

![Buổi thực hành](/images/event6/handsonjob.jpg)
*Thực hành triển khai security*

![QR Code](/images/event6/qrCode.jpg)
*Tài liệu và resources sự kiện*

## Những điểm chính rút ra

- Hiểu biết toàn diện về AWS Security Pillar và 5 lĩnh vực cốt lõi
- Chiến lược kiến trúc IAM và quản lý truy cập thực tế
- Best practices về detection và monitoring với các dịch vụ security AWS
- Các mẫu triển khai bảo vệ infrastructure và data
- Playbooks incident response và kỹ thuật tự động hóa
