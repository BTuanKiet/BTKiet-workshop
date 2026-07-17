---
title: "Week 10 Worklog"
date: 2026-07-08
weight: 2
chapter: false
pre: " <b> 1.10. </b> "
---


### Week 10 Objectives
- Learn and practice Infrastructure as Code (IaC) for containerized applications using AWS CDK.
- Build and operate professional DevOps workflows with fully automated CI/CD pipelines.
- Deploy hybrid storage solutions that efficiently connect on-premise environments to the cloud.
- Set up a high-performance Windows file system using Amazon FSx.

### Daily progress table

| Day | Detailed tasks | Start date | Completion date | Reference materials |
| --- | --- | --- | --- | --- |
| Monday | - Practiced AWS CDK for ECS: created Spring Boot microservices and used CDK V2 to provision ECR, VPC, NAT Gateway, Cluster, and Fargate Service. | 22/06/2026 | 22/06/2026 | [AWS CDK](https://000118.awsstudygroup.com/) |
| Tuesday | - Built a CI/CD pipeline for Amazon EKS: created the necessary IAM Roles and configured CodePipeline for automated code deployment to the EKS cluster. | 23/06/2026 | 23/06/2026 | [CI/CD for EKS](https://000017.awsstudygroup.com/) |
| Wednesday | - Built a CI/CD pipeline for Amazon EC2: used the CodeSuite toolset to deploy a Node.js application to a Linux server.<br>- Set up CI/CD for ECS Containers: configured Container Insights and routed system logs through Firelens. | 24/06/2026 | 25/06/2026 | [CI/CD for EC2](https://000023.awsstudygroup.com/)<br>[CI/CD for ECS](https://000152.awsstudygroup.com/) |
| Friday | - Deployed AWS Storage Gateway: set up a File Storage Gateway connecting directly to an on-premise server.<br>- Provisioned Amazon FSx for Windows: Multi-AZ file system with file share, shadow copies, and throughput scaling configuration. | 26/06/2026 | 27/06/2026 | [AWS Storage Gateway](https://000024.awsstudygroup.com/)<br>[Amazon FSx](https://000025.awsstudygroup.com/) |

### Key achievements
- Infrastructure as Code: learned how to use AWS CDK to consistently synchronize and manage the entire modern infrastructure stack.
- Professional CI/CD operations: successfully built automated deployment pipelines for EKS, ECS, and EC2 using the CodeSuite toolset.
- Flexible hybrid storage: successfully deployed File Storage Gateway to seamlessly sync on-premise data to Amazon S3.
- Modern Windows file system: successfully configured an Amazon FSx architecture with high availability and advanced data deduplication support.
- Pipeline monitoring and security: effectively used CloudWatch Events, KMS, and X-Ray for performance measurement and pipeline security hardening.
