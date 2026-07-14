---
title: "Week 3 Worklog"
date: 2026-07-08
weight: 1
chapter: false
pre: " <b> 1.3. </b> "
---


### Week 3 Objectives
- Improve administration efficiency by using AWS CLI instead of the Console.
- Learn NoSQL storage with Amazon DynamoDB and optimize access speed with Amazon ElastiCache.
- Practice advanced networking concepts such as multi-network connectivity and infrastructure security.
- Deploy a global content delivery solution and process data at the network edge (Edge Computing).

### Daily progress table

| Day | Detailed tasks | Start date | Completion date | Reference materials |
| --- | --- | --- | --- | --- |
| Monday | AWS CLI: configured profiles with access keys, prepared the local environment, and practiced administrative commands for S3, EC2, IAM, and VPC. | 04/05/2026 | 04/05/2026 | [AWS Documentation](https://000011.awsstudygroup.com/) |
| Tuesday | Amazon DynamoDB: studied NoSQL structures such as tables, items, attributes, primary keys, and secondary indexes. Practiced CRUD operations using the console and Python SDK. | 05/05/2026 | 05/05/2026 | [AWS Documentation](https://000060.awsstudygroup.com/) |
| Wednesday | Amazon ElastiCache: created Redis clusters, configured subnet groups, and used the SDK to perform set/get data operations. | 06/05/2026 | 06/05/2026 | [AWS Documentation](https://000061.awsstudygroup.com/) |
| Thursday | Networking workshop: practiced Transit Gateway and Site-to-Site VPN for hybrid cloud connectivity. Configured VPC Peering and VPC Endpoints for AWS services. | 07/05/2026 | 07/05/2026 | [AWS Documentation](https://000092.awsstudygroup.com/) |
| Friday | Amazon CloudFront: created a distribution, configured S3/EC2 origins, set cache behaviors, and customized error pages. | 08/05/2026 | 08/05/2026 | [AWS Documentation](https://000094.awsstudygroup.com/) |
| Saturday | Edge Computing: studied edge locations and deployed a Lambda@Edge function to CloudFront for request processing at the edge. | 09/05/2026 | 10/05/2026 | [AWS Documentation](https://000130.awsstudygroup.com/) |

### Key achievements
- CLI administration: able to use AWS CLI to directly access AWS public APIs and write shell scripts to automate resource provisioning.
- Large-scale data processing: understood how to work with DynamoDB for high I/O needs and unpredictable data structures. Learned how to use ElastiCache for fast temporary caching of small, highly volatile data.
- Complex network design: able to set up private connectivity between VPCs via VPC Peering or centralized connectivity through Transit Gateway. Understood how to use VPC Endpoints to connect to services without going through the public internet.
- Application delivery and acceleration: successfully deployed Amazon CloudFront to cache content at Edge servers, reducing latency and improving availability for web applications.
- Edge programming: first hands-on experience deploying processing logic directly at Edge locations through Lambda@Edge, helping optimize response times for end users.
