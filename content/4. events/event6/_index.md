---
title: "Event 6"
weight: 4.6
chapter: false
pre: " <b> 4.6. </b> "

---

# Event 6 :: AWS Well-Architected Security Pillar

## Event Details
- **Date:** Friday, November 29, 2025
- **Time:** 8:30 AM – 12:00 PM (Morning Only)
- **Location:** AWS Vietnam Office

## 8:30 – 8:50 AM | Opening & Security Foundation
- Security Pillar role in Well-Architected
- Core principles: Least Privilege – Zero Trust – Defense in Depth
- Shared Responsibility Model
- Top threats in Vietnam cloud environment

## ⭐ Pillar 1 — Identity & Access Management
### 8:50 – 9:30 AM | Modern IAM Architecture
- IAM: Users, Roles, Policies – avoid long-term credentials
- IAM Identity Center: SSO, permission sets
- SCP & permission boundaries for multi-account
- MFA, credential rotation, Access Analyzer
- **Mini Demo:** Validate IAM Policy + simulate access

## ⭐ Pillar 2 — Detection
### 9:30 – 9:55 AM | Detection & Continuous Monitoring
- CloudTrail (org-level), GuardDuty, Security Hub
- Logging at all layers: VPC Flow Logs, ALB/S3 logs
- Alerting & automation with EventBridge
- Detection-as-Code (infrastructure + rules)

## 9:55 – 10:10 AM | Coffee Break

## ⭐ Pillar 3 — Infrastructure Protection
### 10:10 – 10:40 AM | Network & Workload Security
- VPC segmentation, private vs public placement
- Security Groups vs NACLs: application model
- WAF + Shield + Network Firewall
- Workload protection: EC2, ECS/EKS security basics

## ⭐ Pillar 4 — Data Protection
### 10:40 – 11:10 AM | Encryption, Keys & Secrets
- KMS: key policies, grants, rotation
- Encryption at-rest & in-transit: S3, EBS, RDS, DynamoDB
- Secrets Manager & Parameter Store — rotation patterns
- Data classification & access guardrails

## ⭐ Pillar 5 — Incident Response
### 11:10 – 11:40 AM | IR Playbook & Automation
- IR lifecycle according to AWS
- **Playbook:**
  - Compromised IAM key
  - S3 public exposure
  - EC2 malware detection
- Snapshot, isolation, evidence collection
- Auto-response using Lambda/Step Functions

## 11:40 – 12:00 PM | Wrap-Up & Q&A
- Summary of 5 pillars
- Common pitfalls & Vietnam enterprise reality
- Security learning roadmap (Security Specialty, SA Pro)

## Event Photos

![Security Workshop](/images/event6/FCJ.jpg)
*AWS Well-Architected Security Pillar Workshop*

![Security Groups](/images/event6/securitygroups.jpg)
*Security Groups configuration*

![KMS Workflow](/images/event6/KMSworkflow.jpg)
*KMS encryption workflow*

![Incident Response](/images/event6/incidentResponse.jpg)
*Incident Response playbook*

![Hands-on Session](/images/event6/handsonjob.jpg)
*Hands-on security implementation*

![QR Code](/images/event6/qrCode.jpg)
*Event resources and materials*

## Key Takeaways

- Comprehensive understanding of AWS Security Pillar and 5 core areas
- Practical IAM architecture and access management strategies
- Detection and monitoring best practices with AWS security services
- Infrastructure and data protection implementation patterns
- Incident response playbooks and automation techniques
