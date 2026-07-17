---
title: "Week 4 Worklog"
date: 2026-07-08
weight: 1
chapter: false
pre: " <b> 1.4. </b> "
---


### Week 4 Objectives
- Learn about serverless computing and workflow automation with AWS Lambda.
- Develop skills for organizing and managing resources at scale using Tags and Resource Groups.
- Strengthen centralized system administration and secure remote access via AWS Systems Manager.
- Optimize infrastructure performance and cost through accurate resource right sizing.

### Daily progress table

| Day | Detailed tasks | Start date | Completion date | Reference materials |
| --- | --- | --- | --- | --- |
| Monday | - Studied serverless concepts and practiced AWS Lambda: wrote a function to automatically start/stop EC2 instances for cost optimization. | 11/05/2026 | 11/05/2026 | [AWS Lambda](https://000022.awsstudygroup.com/) |
| Tuesday | - Applied metadata tagging with Tags & Resource Groups: created resource groups based on Tags for centralized management.<br>- Configured IAM Resource Tags: applied least privilege policies based on Tags to control EC2 access. | 12/05/2026 | 13/05/2026 | [Tags & Resource Groups](https://000027.awsstudygroup.com/)<br>[IAM Resource Tags](https://000028.awsstudygroup.com/) |
| Thursday | - Practiced AWS Systems Manager Patch Manager & Run Command: automated bulk patching and remote command execution across multiple servers.<br>- Practiced SSM Session Manager: remotely connected to Linux/Windows servers (both public and private) without opening SSH/RDP ports. | 14/05/2026 | 15/05/2026 | [SSM Patch Manager](https://000031.awsstudygroup.com/)<br>[SSM Session Manager](https://000058.awsstudygroup.com/) |
| Friday | - Practiced EC2 Right Sizing: used CloudWatch Agent to collect memory metrics and Compute Optimizer to select the optimal EC2 configuration. | 16/05/2026 | 16/05/2026 | [EC2 Right Sizing](https://000032.awsstudygroup.com/) |

### Key achievements
- Operations automation: successfully deployed AWS Lambda to perform scheduled tasks such as starting/stopping servers on a schedule, saving costs for environments that don't need to run 24/7.
- Scientific resource governance: understood how to use Tags to classify resources by purpose, owner, or environment (Dev/Test/Prod). Used Resource Groups to perform bulk operations on grouped resources.
- Decentralized security: successfully applied IAM permission scoping based on Resource Tags, allowing users to only interact with resources that have matching tags.
- Modern system management: used AWS Systems Manager to automate maintenance, patch software, and execute remote commands without managing SSH keys.
- Resource optimization: learned how to use real-world metrics (CPU, RAM) collected from servers to make right-sizing decisions, avoiding wasted resources.
