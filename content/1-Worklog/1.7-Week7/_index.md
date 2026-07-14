---
title: "Week 7 Worklog"
date: 2026-07-08
weight: 1
chapter: false
pre: " <b> 1.7. </b> "
---


### Week 7 Objectives
- Set up application defense layers to block common vulnerability exploits on web applications.
- Deploy data encryption and centralized, secure credential management solutions.
- Automate the discovery of sensitive data and monitor security threats across the entire system.
- Systematize network firewall policy management at multi-account scale.

### Daily progress table

| Day | Detailed tasks | Start date | Completion date | Reference materials |
| --- | --- | --- | --- | --- |
| Monday | AWS Web Application Firewall (WAF): studied Web ACLs and deployed managed rules and custom rules to protect a sample application. | 01/06/2026 | 01/06/2026 | [AWS Documentation](https://000026.awsstudygroup.com/) |
| Tuesday | AWS Key Management Service (KMS): practiced encrypting S3 objects, logged key access through CloudTrail, and queried logs with Athena. | 02/06/2026 | 02/06/2026 | [AWS Documentation](https://000033.awsstudygroup.com/) |
| Wednesday | Amazon Macie: activated the service, scanned S3 buckets for personally identifiable information (PII), and reviewed bucket security. | 03/06/2026 | 03/06/2026 | [AWS Documentation](https://000090.awsstudygroup.com/) |
| Thursday | AWS Secrets Manager: managed RDS passwords, configured secret rotation, and integrated secrets with Fargate. | 04/06/2026 | 04/06/2026 | [AWS Documentation](https://000096.awsstudygroup.com/) |
| Friday | AWS Firewall Manager: audited and centrally restricted security groups, then created security policies for SSH and RDP access. | 05/06/2026 | 05/06/2026 | [AWS Documentation](https://000097.awsstudygroup.com/) |
| Saturday | AWS GuardDuty: studied threat detection and practiced responding to compromised EC2 or IAM credential scenarios. | 06/06/2026 | 07/06/2026 | [AWS Documentation](https://000098.awsstudygroup.com/) |

### Key achievements
- Application layer protection: used AWS WAF to filter malicious requests through managed and custom rule systems.
- Secure data at rest: understood the data encryption process with AWS KMS and access trail control via CloudTrail/Athena.
- Sensitive data control: used Amazon Macie to automatically identify PII data and monitor insecure bucket configurations.
- Credential management: deployed Secrets Manager to eliminate plaintext passwords and implement automatic periodic rotation.
- Centralized network governance: used Firewall Manager to ensure synchronized Security Group compliance across systems.
- Proactive intrusion detection: set up Amazon GuardDuty to analyze system logs and detect abnormal behavior such as crypto mining and credential leaks.
