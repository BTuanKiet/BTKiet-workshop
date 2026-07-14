---
title: "Week 10 Worklog"
date: 2026-07-08
weight: 2
chapter: false
pre: " <b> 1.10. </b> "
---


### Week 10 Objectives
- Learn infrastructure as code (IaC) for container applications through AWS CDK.
- Build and operate professional DevOps workflows with fully automated CI/CD pipelines.
- Deploy a hybrid storage solution that effectively connects on-premises environments and the cloud.
- Set up a high-performance Windows file storage system using Amazon FSx.

### Daily progress table

| Day | Detailed tasks | Start date | Completion date | Reference materials |
| --- | --- | --- | --- | --- |
| Monday | AWS CDK for ECS: created Spring Boot microservices and used CDK v2 to define ECR, VPC, NAT Gateway, cluster, and Fargate service resources. | 22/06/2026 | 22/06/2026 | [AWS Documentation](https://000118.awsstudygroup.com/) |
| Tuesday | CI/CD for Amazon EKS: created the required IAM roles and configured CodePipeline to automatically deploy source code to an EKS cluster. | 23/06/2026 | 23/06/2026 | [AWS Documentation](https://000017.awsstudygroup.com/) |
| Wednesday | CI/CD for Amazon EC2: used the CodeSuite toolset to build a deployment pipeline for a Node.js application on a Linux server. | 24/06/2026 | 24/06/2026 | [AWS Documentation](https://000023.awsstudygroup.com/) |
| Thursday | CI/CD for ECS Containers: completed the pipeline setup, configured Container Insights, and routed system logs through FireLens. | 25/05/2026 | 25/06/2026 | [AWS Documentation](https://000152.awsstudygroup.com/) |
| Friday | AWS Storage Gateway: prepared S3 and EC2 environments, then deployed File Storage Gateway connected to an on-premises server. | 26/06/2026 | 26/06/2026 | [AWS Documentation](https://000024.awsstudygroup.com/) |
| Saturday - Sunday | Amazon FSx for Windows: created a Multi-AZ file system, configured file shares and shadow copies, and expanded throughput capacity. | 27/06/2026 | 28/06/2026 | [AWS Documentation](https://000025.awsstudygroup.com/) |

### Key achievements
- Infrastructure as code: learned to use AWS CDK to manage the entire modern infrastructure consistently as code.
- CI/CD operations: successfully built automated deployment pipelines on EKS, ECS, and EC2 using the CodeSuite toolset.
- Hybrid storage: successfully deployed File Storage Gateway to synchronize data from on-premises infrastructure to Amazon S3.
- Windows file system: set up a high-availability Amazon FSx architecture supporting data deduplication.
- Pipeline monitoring and security: used CloudWatch Events, KMS, and X-Ray for performance measurement and pipeline security.
