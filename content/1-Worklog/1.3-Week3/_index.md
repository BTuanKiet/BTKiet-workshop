---
title: "Week 3 Worklog"
date: 2026-07-08
weight: 1
chapter: false
pre: " <b> 1.3. </b> "
---


### Week 3 Objectives
- Improve administration efficiency using the AWS CLI instead of the Console.
- Learn NoSQL storage with Amazon DynamoDB and optimize access speed with Amazon ElastiCache.
- Practice advanced networking concepts such as multi-network connectivity and infrastructure security.
- Deploy a global content distribution solution and process data at the network edge (Edge Computing).

### Daily progress table

| Day | Detailed tasks | Start date | Completion date | Reference materials |
| --- | --- | --- | --- | --- |
| Monday | - Learned and practiced AWS CLI: configured Profiles (Access Key/Secret Key), set up the environment, and ran management commands for S3, EC2, IAM, and VPC. | 04/05/2026 | 04/05/2026 | [AWS CLI](https://000011.awsstudygroup.com/) |
| Tuesday | - Studied Amazon DynamoDB: NoSQL structure (Tables, Items, Attributes), Primary Keys, and Secondary Indexes. Practiced CRUD operations via Console and Python SDK. | 05/05/2026 | 05/05/2026 | [Amazon DynamoDB](https://000060.awsstudygroup.com/) |
| Wednesday | - Practiced Amazon ElastiCache: created a Redis Cluster (enabled/disabled modes), configured a Subnet Group, and used the SDK for Set/Get operations. | 06/05/2026 | 06/05/2026 | [Amazon ElastiCache](https://000061.awsstudygroup.com/) |
| Thursday | - Practiced Networking Workshop: Transit Gateway, Site-to-Site VPN for Hybrid Cloud connectivity. Configured VPC Peering and set up VPC Endpoints for AWS services. | 07/05/2026 | 07/05/2026 | [Networking on AWS](https://000092.awsstudygroup.com/) |
| Friday | - Created an Amazon CloudFront Distribution, configured origins (S3/EC2), set up Cache Behaviors, and customized error pages.<br>- Studied Edge Locations and practiced deploying Lambda@Edge functions to CloudFront for edge-side request handling. | 08/05/2026 | 09/05/2026 | [Amazon CloudFront](https://000094.awsstudygroup.com/)<br>[Lambda@Edge](https://000130.awsstudygroup.com/) |

### Key achievements
- CLI-based administration: can use the AWS CLI to access AWS public APIs directly and write shell scripts to automate resource deployment.
- Large-scale data processing: understood how to operate DynamoDB for high I/O demands and unpredictable data structures. Learned how to use ElastiCache for fast temporary storage of small, highly volatile data.
- Complex network design: able to establish private connectivity between VPCs via VPC Peering or centralized routing through Transit Gateway. Understood how to use VPC Endpoints to connect services without going through the public internet.
- Application distribution and acceleration: successfully deployed Amazon CloudFront to cache content on Edge servers, reducing latency and improving web application availability.
- Edge programming: began working with Lambda@Edge to deploy processing logic at Edge locations, optimizing customer response times.
