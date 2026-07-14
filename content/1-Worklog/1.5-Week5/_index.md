---
title: "Week 5 Worklog"
date: 2026-07-08
weight: 1
chapter: false
pre: " <b> 1.5. </b> "
---


### Week 5 Objectives
- Strengthen network infrastructure monitoring and security through traffic analysis.
- Learn financial administration, billing delegation, and service quota management on AWS.
- Apply best practices for controlling resource usage costs.
- Automate data protection workflows and deploy anomaly detection systems for backups.

### Daily progress table

| Day | Detailed tasks | Start date | Completion date | Reference materials |
| --- | --- | --- | --- | --- |
| Monday | VPC Flow Logs: learned how to enable IP traffic logs and exported data to CloudWatch Logs to diagnose security group rules. | 18/05/2026 | 18/05/2026 | [AWS Documentation](https://000074.awsstudygroup.com/) |
| Tuesday | Billing access delegation: practiced enabling IAM user/group access to the billing console from the root account. | 19/05/2026 | 19/05/2026 | [AWS Documentation](https://000075.awsstudygroup.com/) |
| Wednesday | Service Quotas: learned how to review service limits and submit quota increase requests to AWS Support. | 20/05/2026 | 20/05/2026 | [AWS Documentation](https://000063.awsstudygroup.com/) |
| Thursday | Cost governance with IAM: created policies to restrict usage by Region, EC2 family, and EBS volume size. | 21/05/2026 | 21/05/2026 | [AWS Documentation](https://000064.awsstudygroup.com/) |
| Friday | Data Lifecycle Manager (DLM): configured policies to automate EBS snapshot creation, retention, and deletion. | 22/05/2026 | 22/05/2026 | [AWS Documentation](https://000088.awsstudygroup.com/) |
| Saturday | EBS Backup Anomaly Detection: built a serverless pipeline with Lambda, EventBridge, and CloudWatch to detect unusual snapshot changes. | 23/05/2026 | 24/05/2026 | [AWS Documentation](https://000089.awsstudygroup.com/) |

### Key achievements
- Network traffic transparency: understood how to use VPC Flow Logs to monitor inbound/outbound traffic without impacting network performance, helping identify unauthorized access or misconfigured firewalls.
- Secure financial governance: learned how to separate permissions, allowing accounting staff or managers to access Billing data without needing to share root account credentials.
- Proactive resource management: learned how to use Service Quotas to plan infrastructure scaling and avoid service interruptions caused by hitting AWS default limits.
- Cost optimization: successfully applied IAM Policies to prevent wasteful resource provisioning.
- Automated data protection: deployed Amazon DLM to maintain long-term backup compliance (over 90 days) at optimal cost through Snapshot Archive.
- Early threat detection: set up CloudWatch's anomaly detection mechanism to send SNS alerts when sudden changes occur in backup data.
