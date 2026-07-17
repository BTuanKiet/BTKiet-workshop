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
| Monday | - Enabled and studied VPC Flow Logs: exported traffic data to CloudWatch Logs to diagnose Security Group rules. | 18/05/2026 | 18/05/2026 | [VPC Flow Logs](https://000074.awsstudygroup.com/) |
| Tuesday | - Practiced billing delegation: granted IAM User/Group access to the billing console from the root account.<br>- Studied Service Quotas: reviewed service limits and learned the process for submitting quota increase requests to AWS Support. | 19/05/2026 | 20/05/2026 | [AWS Billing](https://000075.awsstudygroup.com/)<br>[Service Quotas](https://000063.awsstudygroup.com/) |
| Wednesday | - Practiced cost governance with IAM: configured policies to restrict usage by Region, EC2 instance family, and EBS volume size. | 21/05/2026 | 21/05/2026 | [IAM Cost Management](https://000064.awsstudygroup.com/) |
| Friday | - Configured Data Lifecycle Manager (DLM): automated EBS snapshot creation, retention, and deletion.<br>- Built EBS Backup Anomaly Detection: a serverless pipeline (Lambda, EventBridge, CloudWatch) to detect unusual changes between snapshots. | 22/05/2026 | 23/05/2026 | [Data Lifecycle Manager](https://000088.awsstudygroup.com/)<br>[EBS Backup Anomaly Detection](https://000089.awsstudygroup.com/) |

### Key achievements
- Network traffic transparency: understood how to use VPC Flow Logs to monitor inbound/outbound traffic without impacting network performance, helping identify unauthorized access or misconfigured firewalls.
- Secure financial governance: learned how to separate permissions, allowing accounting staff or managers to access Billing data without sharing root account credentials.
- Proactive resource management: learned how to use Service Quotas to plan infrastructure scaling and avoid service interruptions caused by hitting AWS default limits.
- Cost optimization: successfully applied IAM Policies to prevent wasteful resource provisioning.
- Automated data protection: deployed Amazon DLM to maintain long-term backup compliance (over 90 days) at optimal cost through Snapshot Archive.
- Early threat detection: set up CloudWatch's anomaly detection mechanism to send SNS alerts when sudden changes occur in backup data.
