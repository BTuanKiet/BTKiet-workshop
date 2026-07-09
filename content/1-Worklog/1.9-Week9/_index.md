---
title: "Week 9 Worklog"
date: 2026-07-08
weight: 1
chapter: false
pre: " <b> 1.9. </b> "
---


### Week 9 Objectives
- Learn event-driven architecture using SQS and SNS.
- Explore shared storage and high availability concepts.
- Practice containerization and orchestration with Docker and ECS.

### Daily progress table

| Day | Detailed tasks | Start date | Completion date | Reference materials |
| --- | --- | --- | --- | --- |
| Monday | SQS and SNS: built a pub/sub architecture and practiced message filtering to route events to specific subscribers. | 15/06/2026 | 15/06/2026 | AWS Documentation |
| Tuesday | EBS Multi-Attach: attached multiple EC2 instances to one EBS volume and used NVMe reservations to coordinate access. | 16/06/2026 | 16/06/2026 | AWS Documentation |
| Wednesday | MySQL HA Cluster: deployed a MySQL cluster using Multi-Attach and LVM, then automated recovery with a Systems Manager runbook. | 17/06/2026 | 17/06/2026 | AWS Documentation |
| Thursday | Windows Failover Cluster: configured a failover cluster, cluster quorum, and shared storage components. | 18/06/2026 | 18/06/2026 | AWS Documentation |
| Friday | Docker: containerized a sample application, used Docker Compose to manage containers, and pushed the completed image to Amazon ECR. | 19/06/2026 | 19/06/2026 | AWS Documentation |
| Saturday - Sunday | Amazon ECS: created a cluster and task definitions, configured an Application Load Balancer, and deployed a blue/green model. | 20/06/2026 | 21/06/2026 | AWS Documentation |

### Main activities
- Built a pub/sub messaging pattern with Amazon SQS and SNS.
- Studied EBS Multi-Attach for shared storage and MySQL high-availability clusters.
- Worked with Windows Failover Clustering and internal load balancers.
- Packaged applications with Docker, pushed images to Amazon ECR, and deployed them on ECS.

### Key achievements
- Better understood how event-driven systems improve decoupling and scalability.
- Learned how to design highly available and fault-tolerant architectures.
- Gained practical familiarity with container-based deployment workflows on AWS.
