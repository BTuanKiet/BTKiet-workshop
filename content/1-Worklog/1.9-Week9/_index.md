---
title: "Week 9 Worklog"
date: 2026-07-08
weight: 1
chapter: false
pre: " <b> 1.9. </b> "
---


### Week 9 Objectives
- Learn and practice building event-driven architecture to allow services to operate independently.
- Deploy high-performance shared storage solutions with Amazon EBS Multi-Attach.
- Build high-availability systems for MySQL and Windows Failover Cluster.
- Learn container technology (Docker) and orchestration through Amazon ECS.

### Daily progress table

| Day | Detailed tasks | Start date | Completion date | Reference materials |
| --- | --- | --- | --- | --- |
| Monday | SQS and SNS: built a pub/sub architecture and practiced message filtering to route events to specific subscribers. | 15/06/2026 | 15/06/2026 | [AWS Documentation](https://000077.awsstudygroup.com/) |
| Tuesday | EBS Multi-Attach: attached multiple EC2 instances to one EBS volume and used NVMe reservations to coordinate access. | 16/06/2026 | 16/06/2026 | [AWS Documentation](https://100000.awsstudygroup.com/) |
| Wednesday | MySQL HA Cluster: deployed a MySQL cluster using Multi-Attach and LVM, then automated recovery with a Systems Manager runbook. | 17/06/2026 | 17/06/2026 | [AWS Documentation](https://100001.awsstudygroup.com/) |
| Thursday | Windows Failover Cluster: configured a failover cluster, cluster quorum, and shared storage components. | 18/06/2026 | 18/06/2026 | [AWS Documentation](https://100002.awsstudygroup.com/) |
| Friday | Docker: containerized a sample application, used Docker Compose to manage containers, and pushed the completed image to Amazon ECR. | 19/06/2026 | 19/06/2026 | [AWS Documentation](https://000015.awsstudygroup.com/) |
| Saturday - Sunday | Amazon ECS: created a cluster and task definitions, configured an Application Load Balancer, and deployed a blue/green model. | 20/06/2026 | 21/06/2026 | [AWS Documentation](https://000016.awsstudygroup.com/) |

### Key achievements
- Flexible system design: understood how to combine SNS and SQS to queue messages and reduce complex direct routing logic.
- Shared storage: successfully deployed a solution for secure data sharing between Linux/Windows servers using EBS Multi-Attach volumes.
- Fault-tolerant system design: worked with the Windows Failover Cluster and combined Internal NLB synchronized with the MySQL Cluster.
- Containerized deployment: learned the process of packaging source code with Docker and managing containers through Amazon ECS.
- Operations automation: used Systems Manager and Incident Manager to set up response scenarios and reduce system incident resolution time (MTTR).
