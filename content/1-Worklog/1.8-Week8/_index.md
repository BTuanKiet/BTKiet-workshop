---
title: "Week 8 Worklog"
date: 2026-07-08
weight: 1
chapter: false
pre: " <b> 1.8. </b> "
---


### Week 8 Objectives
- Automate OS patching processes to minimize security vulnerabilities and operational burden.
- Deploy a centralized authentication and identity management solution for web and mobile applications.
- Apply security best practices to protect data on Amazon S3.
- Establish a comprehensive data protection mechanism and automate resource recovery.
- Learn advanced network connectivity methods, from point-to-point connections to centralized network governance models.

### Daily progress table

| Day | Detailed tasks | Start date | Completion date | Reference materials |
| --- | --- | --- | --- | --- |
| Monday | EC2 Image Builder: built an automated pipeline to create AMIs with the latest patches using a blue/green deployment approach. | 08/06/2026 | 08/06/2026 | [AWS Documentation](https://000099.awsstudygroup.com/) |
| Tuesday | Amazon Cognito: studied User Pools and Identity Pools, and practiced implementing federated authentication for an application. | 09/06/2026 | 09/06/2026 | [AWS Documentation](https://000141.awsstudygroup.com/) |
| Wednesday | S3 Security Best Practices: configured Block Public Access (BPA), required HTTPS and SSE-S3 encryption, and used Access Analyzer. | 10/06/2026 | 10/06/2026 | [AWS Documentation](https://000069.awsstudygroup.com/) |
| Thursday | AWS Backup: created a backup plan to automate backups for EBS, RDS, and DynamoDB, and configured SNS notifications. | 11/06/2026 | 11/06/2026 | [AWS Documentation](https://000013.awsstudygroup.com/) |
| Friday | VPC Peering: created direct private IP connectivity between VPCs and enabled cross-peer DNS resolution. | 12/06/2026 | 12/06/2026 | [AWS Documentation](https://000019.awsstudygroup.com/) |
| Saturday - Sunday | AWS Transit Gateway: deployed a hub-and-spoke model to connect multiple VPCs through a single centralized gateway. | 13/06/2026 | 14/06/2026 | [AWS Documentation](https://000020.awsstudygroup.com/) |

### Key achievements
- Secure system operations: used EC2 Image Builder combined with Systems Manager to automate AMI packaging and patch updates.
- Identity solution: clearly understood how to use Cognito User Pools and Identity Pools for managing and authorizing customer access.
- Tightened storage security: successfully applied advanced security guardrails to S3 through AWS Config policies and VPC Endpoint Policies.
- Automated digital asset protection: deployed AWS Backup to centralize the entire backup workflow and ensure rapid data recovery.
- Network infrastructure: successfully set up VPC Peering for simple connections and deployed AWS Transit Gateway to simplify multi-VPC network architecture.
