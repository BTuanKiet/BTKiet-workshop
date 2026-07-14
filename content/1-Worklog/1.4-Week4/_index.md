---
title: "Week 4 Worklog"
date: 2026-07-08
weight: 1
chapter: false
pre: " <b> 1.4. </b> "
---


### Week 4 Objectives
- Learn serverless computing and process automation with AWS Lambda.
- Develop skills to organize and manage resources at scale using Tags and Resource Groups.
- Enhance centralized system administration and secure remote access through AWS Systems Manager.
- Optimize infrastructure performance and cost through accurate resource right-sizing.

### Daily progress table

| Day | Detailed tasks | Start date | Completion date | Reference materials |
| --- | --- | --- | --- | --- |
| Monday | AWS Lambda: studied serverless computing and wrote a function to automatically start and stop EC2 instances for cost optimization. | 11/05/2026 | 11/05/2026 | [AWS Documentation](https://000022.awsstudygroup.com/) |
| Tuesday | Tags and Resource Groups: added metadata tags to resources and created tag-based resource groups for centralized management. | 12/05/2026 | 12/05/2026 | [AWS Documentation](https://000027.awsstudygroup.com/) |
| Wednesday | IAM Resource Tags: configured least-privilege policies based on tags to control EC2 access permissions. | 13/05/2026 | 13/05/2026 | [AWS Documentation](https://000028.awsstudygroup.com/) |
| Thursday | AWS Systems Manager (SSM): practiced Patch Manager for batch patching and Run Command for executing commands across multiple servers. | 14/05/2026 | 14/05/2026 | [AWS Documentation](https://000031.awsstudygroup.com/) |
| Friday | SSM Session Manager: connected remotely to Linux and Windows servers, both public and private, without opening SSH/RDP ports. | 15/05/2026 | 15/05/2026 | [AWS Documentation](https://000058.awsstudygroup.com/) |
| Saturday | EC2 Right Sizing: used CloudWatch Agent to collect memory data and Compute Optimizer to choose optimized EC2 configurations. | 16/05/2026 | 17/05/2026 | [AWS Documentation](https://000032.awsstudygroup.com/) |

### Key achievements
- Operations automation: successfully deployed AWS Lambda to perform scheduled tasks such as starting/stopping servers on a schedule, saving costs for environments that don't need to run 24/7.
- Systematic resource management: understood how to use Tags as a tool to classify resources by purpose, owner, or environment (Dev/Test/Prod). Used Resource Groups to perform batch operations on these resource groups.
- Decentralized security: successfully applied IAM permission assignments based on Resource Tags, only allowing users to interact with resources that have matching tags.
- Modern system management: used AWS Systems Manager to automate maintenance, software patching, and remote command execution without managing SSH keys, strengthening infrastructure security.
- Resource optimization: learned how to use actual metrics (CPU, RAM) collected from servers to make informed Instance type change decisions (Right Sizing), avoiding wasted resources.
