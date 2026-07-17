---
title: "Week 8 Worklog"
date: 2026-07-08
weight: 1
chapter: false
pre: " <b> 1.8. </b> "
---


### Week 8 Objectives
- Automate OS patching workflows to reduce security vulnerabilities and operational overhead.
- Deploy centralized authentication and identity management for web and mobile applications.
- Apply security standards to protect data on Amazon S3.
- Set up comprehensive data protection mechanisms and automate resource recovery.
- Learn advanced network connectivity methods, from point-to-point connections to centralized network management.

### Daily progress table

| Day | Detailed tasks | Start date | Completion date | Reference materials |
| --- | --- | --- | --- | --- |
| Monday | - Practiced EC2 Image Builder: built a pipeline to automatically create AMIs containing the latest patches using the Blue/Green Deployment approach. | 08/06/2026 | 08/06/2026 | [EC2 Image Builder](https://000099.awsstudygroup.com/) |
| Tuesday | - Studied Amazon Cognito: User Pools and Identity Pools. Practiced deploying federated authentication for an application. | 09/06/2026 | 09/06/2026 | [Amazon Cognito](https://000141.awsstudygroup.com/) |
| Wednesday | - Applied S3 Security Best Practices: configured Block Public Access (BPA), enforced HTTPS/SSE-S3 encryption, and used Access Analyzer.<br>- Created an AWS Backup Plan: automated backups of EBS, RDS, and DynamoDB resources, and configured SNS notifications. | 10/06/2026 | 11/06/2026 | [S3 Security](https://000069.awsstudygroup.com/)<br>[AWS Backup](https://000013.awsstudygroup.com/) |
| Friday | - Set up VPC Peering: established direct Private IP connectivity between VPCs and enabled Cross-Peer DNS resolution.<br>- Deployed AWS Transit Gateway in Hub-and-Spoke topology: connected multiple VPCs simultaneously through a single centralized gateway. | 12/06/2026 | 13/06/2026 | [VPC Peering](https://000019.awsstudygroup.com/)<br>[AWS Transit Gateway](https://000020.awsstudygroup.com/) |

### Key achievements
- Secure system operations: learned how to use EC2 Image Builder combined with Systems Manager to automate AMI packaging and patch updates.
- Identity solution: clearly understood how to use Cognito User Pools and Identity Pools to manage and authorize customer access.
- Tightened storage security: successfully applied advanced S3 security barriers through AWS Config policies and VPC Endpoint Policies.
- Automated digital asset protection: deployed AWS Backup to centralize the entire backup process and ensure rapid data recovery.
- Network infrastructure optimization: successfully configured VPC Peering for simple connections and deployed AWS Transit Gateway to simplify multi-VPC network architecture.
