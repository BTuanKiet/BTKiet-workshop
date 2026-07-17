---
title: "Week 7 Worklog"
date: 2026-07-08
weight: 1
chapter: false
pre: " <b> 1.7. </b> "
---


### Week 7 Objectives
- Set up web application defense layers to block common vulnerability exploits.
- Deploy data encryption solutions and centralized, secure credential management.
- Automate sensitive data discovery and monitor security threats across the entire system.
- Systematize network firewall policy management at multi-account scale.

### Daily progress table

| Day | Detailed tasks | Start date | Completion date | Reference materials |
| --- | --- | --- | --- | --- |
| Monday | - Studied AWS WAF Web ACLs and practiced deploying Managed Rules and Custom Rules to protect a sample application. | 01/06/2026 | 01/06/2026 | [AWS WAF](https://000026.awsstudygroup.com/) |
| Tuesday | - Practiced AWS KMS: encrypted S3 Objects, logged key access via CloudTrail, and queried logs with Athena. | 02/06/2026 | 02/06/2026 | [AWS KMS](https://000033.awsstudygroup.com/) |
| Wednesday | - Enabled Amazon Macie: scanned S3 buckets to detect personally identifiable information (PII) and assessed bucket security configurations.<br>- Practiced AWS Secrets Manager: managed RDS passwords, configured automatic Secret Rotation, and integrated with Fargate. | 03/06/2026 | 04/06/2026 | [Amazon Macie](https://000090.awsstudygroup.com/)<br>[AWS Secrets Manager](https://000096.awsstudygroup.com/) |
| Friday | - Practiced AWS Firewall Manager: audited and centrally restricted Security Groups, and configured security policies for SSH and RDP.<br>- Studied AWS GuardDuty: learned threat detection mechanisms and handled scenarios involving compromised EC2 instances or IAM credentials. | 05/06/2026 | 06/06/2026 | [AWS Firewall Manager](https://000097.awsstudygroup.com/)<br>[AWS GuardDuty](https://000098.awsstudygroup.com/) |

### Key achievements
- Application layer protection: used AWS WAF to filter malicious requests through managed and custom rule sets.
- Storage data safety: thoroughly understood the data encryption process with AWS KMS and comprehensive access trail control via CloudTrail/Athena.
- Sensitive data control: used Amazon Macie to automatically identify PII data and closely monitor insecure bucket configurations.
- Credential management: deployed Secrets Manager to eliminate plaintext passwords and implement automatic rotation.
- Centralized network governance: used Firewall Manager to ensure synchronized Security Group compliance at scale.
- Intrusion detection: operated Amazon GuardDuty to analyze log data and quickly detect anomalous behavior (cryptomining, credential leaks).
