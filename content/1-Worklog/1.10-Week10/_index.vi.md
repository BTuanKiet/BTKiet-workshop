---
title: "Worklog Tuần 10"
date: 2026-07-08
weight: 2
chapter: false
pre: " <b> 1.10. </b> "
---


### Mục tiêu tuần 10
- Tìm hiểu thực hành hạ tầng dưới dạng mã (IaC) cho các ứng dụng container thông qua AWS CDK.
- Xây dựng và vận hành quy trình DevOps chuyên nghiệp với đường dẫn CI/CD hoàn toàn tự động.
- Triển khai giải pháp lưu trữ lai kết nối hiệu quả giữa môi trường On-premise và đám mây.
- Thiết lập hệ thống lưu trữ tập tin Windows hiệu suất cao sử dụng Amazon FSx.

### Bảng tiến độ theo ngày

| Ngày | Công việc chi tiết | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| Thứ 2 | - Thực hành AWS CDK cho ECS: tạo Microservices Spring Boot và dùng CDK V2 thiết lập ECR, VPC, NAT Gateway, Cluster và Fargate Service. | 22/06/2026 | 22/06/2026 | [AWS CDK](https://000118.awsstudygroup.com/vi/) |
| Thứ 3 | - Xây dựng CI/CD cho Amazon EKS: tạo IAM Role cần thiết, cấu hình CodePipeline triển khai tự động mã nguồn lên cụm EKS. | 23/06/2026 | 23/06/2026 | [CI/CD cho EKS](https://000017.awsstudygroup.com/vi/) |
| Thứ 4 | - Xây dựng CI/CD cho Amazon EC2: dùng bộ CodeSuite để triển khai ứng dụng Node.js lên máy chủ Linux.<br>- Thiết lập CI/CD cho ECS Container: cấu hình Container Insights và định tuyến hệ thống log qua Firelens. | 24/06/2026 | 25/06/2026 | [CI/CD cho EC2](https://000023.awsstudygroup.com/vi/)<br>[CI/CD cho ECS](https://000152.awsstudygroup.com/vi/) |
| Thứ 6 | - Triển khai AWS Storage Gateway: File Storage Gateway kết nối trực tiếp với máy chủ On-premise.<br>- Khởi tạo Amazon FSx cho Windows: Multi-AZ file system, thiết lập file share, shadow copies và cấu hình mở rộng thông lượng. | 26/06/2026 | 27/06/2026 | [AWS Storage Gateway](https://000024.awsstudygroup.com/vi/)<br>[Amazon FSx](https://000025.awsstudygroup.com/vi/) |

### Kết quả đạt được tuần 10
- Hạ tầng dưới dạng mã: Biết cách sử dụng AWS CDK để đồng bộ hóa và quản lý toàn bộ hệ thống hạ tầng hiện đại một cách nhất quán.
- Vận hành CI/CD chuyên nghiệp: Xây dựng thành công các đường dẫn tự động hóa quy trình triển khai mã nguồn trên EKS, ECS hoặc EC2 qua bộ công cụ CodeSuite.
- Lưu trữ lai linh hoạt: Triển khai thành công File Storage Gateway giúp dữ liệu từ hạ tầng cục bộ (On-premise) đồng bộ liền mạch sang Amazon S3.
- Hệ thống tệp Windows hiện đại: Thiết lập thành công kiến trúc Amazon FSx có tính sẵn sàng cao, hỗ trợ các cơ chế chống trùng lặp dữ liệu nâng cao.
- Giám sát và bảo mật Pipeline: Sử dụng hiệu quả CloudWatch Events, KMS và X-Ray phục vụ cho công tác đo lường hiệu năng và tăng cường bảo mật cho pipeline.
