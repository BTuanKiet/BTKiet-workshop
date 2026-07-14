---
title: "Week 11 Worklog"
date: 2026-07-08
weight: 2
chapter: false
pre: " <b> 1.11. </b> "
---


### Week 11 Objectives
- Learn advanced design patterns and build global-scale application architectures with Amazon DynamoDB.
- Develop skills to orchestrate complex workflows between services through AWS Step Functions.
- Practice optimizing storage infrastructure performance for Amazon S3 and Amazon EFS.
- Apply long-term cost optimization strategies through Savings Plans and Reserved Instances models.
- Build a platform for in-depth analysis of cost-related data using AWS Glue and Amazon Athena.

### Daily progress table

| Day | Detailed tasks | Start date | Completion date | Reference materials |
| --- | --- | --- | --- | --- |
| Monday | Amazon DynamoDB Immersion Day: studied advanced application design and completed hands-on lab modules including LHOL, LADV, LGME, and LMR. | 29/06/2026 | 29/06/2026 | [AWS Documentation](https://000039.awsstudygroup.com/) |
| Tuesday | AWS Step Functions: practiced workflow orchestration, worked with processing states, and configured retry/catch error handling. | 30/06/2026 | 30/06/2026 | [AWS Documentation](https://000047.awsstudygroup.com/) |
| Wednesday | Storage Performance Workshop: optimized throughput for small files on S3 and configured high-performance mode for Amazon EFS. | 01/07/2026 | 01/07/2026 | [AWS Documentation](https://000068.awsstudygroup.com/) |
| Thursday | Cost optimization: analyzed Savings Plan types and practiced purchasing and applying cost-saving options for EC2 and RDS services. | 02/07/2026 | 02/07/2026 | [AWS Documentation](https://000042.awsstudygroup.com/) |
| Friday | Cost visualization: deployed account-level cost monitoring, reviewed SP and RI coverage rates, and exported custom EC2 report templates. | 03/07/2026 | 03/07/2026 | [AWS Documentation](https://000034.awsstudygroup.com/) |
| Saturday - Sunday | Advanced cost data analysis: built a collection and analytics platform for detailed cost reports using AWS Glue and Amazon Athena. | 04/07/2026 | 05/07/2026 | [AWS Documentation](https://000040.awsstudygroup.com/) |

### Key achievements
- Advanced DynamoDB patterns: learned to apply design patterns such as Sparse GSI and Composite Keys, and configured Global Tables to achieve millisecond latency.
- Workflow orchestration: successfully built complex State Machines with parallel task processing through Step Functions.
- Storage performance: applied read/write parallelization techniques on Amazon S3 (reaching 55,000 requests/s) and configured EFS to reach over 10 GiBps throughput.
- Cost management: applied Savings Plans and Reserved Instances models following the AWS Well-Architected Framework.
- Cost data analysis: built an automated platform for analyzing detailed cost reports (CUR), enabling flexible queries for infrastructure optimization.
