---
title: "Week 9 Worklog"
date: 2026-07-08
weight: 1
chapter: false
pre: " <b> 1.9. </b> "
---


### Week 9 Objectives
- Learn and practice building event-driven architectures that allow services to operate independently.
- Deploy high-performance shared storage with Amazon EBS Multi-Attach.
- Build highly available (HA) systems for MySQL and Windows Failover Cluster.
- Learn container technology (Docker) and orchestration via Amazon ECS.

### Daily progress table

| Day | Detailed tasks | Start date | Completion date | Reference materials |
| --- | --- | --- | --- | --- |
| Monday | - Built a Pub/Sub architecture with Amazon SQS & SNS. Practiced Message Filtering to route messages directly to specific subscribers. | 15/06/2026 | 15/06/2026 | [Amazon SQS & SNS](https://000077.awsstudygroup.com/) |
| Tuesday | - Practiced EBS Multi-Attach: connected multiple EC2 instances to a single EBS volume simultaneously and used NVMe Reservation to coordinate access. | 16/06/2026 | 16/06/2026 | [EBS Multi-Attach](https://100000.awsstudygroup.com/) |
| Wednesday | - Deployed a MySQL HA Cluster using Multi-Attach and LVM. Automated the incident recovery process via Systems Manager Runbook. | 17/06/2026 | 17/06/2026 | [MySQL HA Cluster](https://100001.awsstudygroup.com/) |
| Thursday | - Set up a Windows Failover Cluster: configured Cluster Quorum and Shared Storage components. | 18/06/2026 | 18/06/2026 | [Windows Failover Cluster](https://100002.awsstudygroup.com/) |
| Friday | - Packaged an application with Docker, managed containers using Docker Compose, and pushed the image to Amazon ECR.<br>- Created an Amazon ECS Cluster, defined Task Definitions, configured an Application Load Balancer, and deployed using the Blue/Green model. | 19/06/2026 | 20/06/2026 | [Docker & Amazon ECR](https://000015.awsstudygroup.com/)<br>[Amazon ECS](https://000016.awsstudygroup.com/) |

### Key achievements
- Flexible system design: understood how to combine SNS and SQS to queue messages and significantly reduce complex direct routing logic.
- Shared storage: successfully deployed a safe data-sharing solution between Linux/Windows servers using EBS Multi-Attach.
- Fault-tolerant system design: effectively operated a Windows Failover Cluster and integrated an Internal NLB with the MySQL Cluster.
- Modernization with containers: learned how to package code with Docker and manage/orchestrate containers flexibly via Amazon ECS.
- Automated operations: effectively used Systems Manager and Incident Manager to configure response playbooks and minimize system incident resolution time (MTTR).
