---
title: "Worklog Tuần 10"
date: 2026-07-08
weight: 2
chapter: false
pre: " <b> 1.10. </b> "
---


### Mục tiêu tuần 10
- Nắm vững thực hành hạ tầng dưới dạng mã (IaC) cho các ứng dụng container thông qua AWS CDK.
- Xây dựng và vận hành quy trình DevOps chuyên nghiệp với đường dẫn CI/CD hoàn toàn tự động.
- Triển khai giải pháp lưu trữ lai kết nối hiệu quả giữa môi trường On-premise và đám mây.
- Thiết lập hệ thống lưu trữ tập tin Windows hiệu suất cao sử dụng Amazon FSx.

### Bảng tiến độ theo ngày

| Ngày | Công việc chi tiết | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| Thứ 2 | AWS CDK cho ECS: Tạo Microservices Spring Boot; Sử dụng công cụ CDK V2 thiết lập toàn diện ECR, VPC, NAT Gateway, Cluster và Fargate Service. | 22/06/2026 | 22/06/2026 | [AWS Documentation](https://000118.awsstudygroup.com/vi/) |
| Thứ 3 | CI/CD cho Amazon EKS: Tạo các IAM Role cần thiết, cấu hình CodePipeline hỗ trợ triển khai tự động mã nguồn lên cụm EKS. | 23/06/2026 | 23/06/2026 | [AWS Documentation](https://000017.awsstudygroup.com/vi/) |
| Thứ 4 | CI/CD cho Amazon EC2: Sử dụng trọn bộ dịch vụ CodeSuite để xây dựng quy trình triển khai ứng dụng Node.js lên máy chủ Linux. | 24/06/2026 | 24/06/2026 | [AWS Documentation](https://000023.awsstudygroup.com/vi/) |
| Thứ 5 | CI/CD cho ECS Container: Thiết lập hoàn chỉnh Pipeline; Tiến hành cấu hình Container Insights và định tuyến hệ thống log qua Firelens. | 25/05/2026 | 25/06/2026 | [AWS Documentation](https://000152.awsstudygroup.com/vi/) |
| Thứ 6 | AWS Storage Gateway: Chuẩn bị môi trường S3 và EC2; Tiến hành triển khai File Storage Gateway kết nối trực tiếp với máy chủ On-premise. | 26/06/2026 | 26/06/2026 | [AWS Documentation](https://000024.awsstudygroup.com/vi/) |
| Thứ 7 - CN | Amazon FSx cho Windows: Khởi tạo Multi-AZ file system; Thiết lập tính năng file share, shadow copies và thực hiện cấu hình mở rộng thông lượng. | 27/06/2026 | 28/06/2026 | [AWS Documentation](https://000025.awsstudygroup.com/vi/) |

### Kết quả đạt được tuần 10
- Làm chủ IaC: Thành thạo sử dụng AWS CDK giúp đồng bộ hóa và quản lý toàn bộ hệ thống hạ tầng hiện đại dưới dạng mã một cách nhất quán.
- Vận hành CI/CD chuyên nghiệp: Xây dựng thành công các đường dẫn tự động hóa quy trình triển khai mã nguồn trên EKS, ECS hoặc EC2 qua bộ công cụ CodeSuite.
- Lưu trữ lai linh hoạt: Triển khai thành công File Storage Gateway giúp dữ liệu từ hạ tầng cục bộ (On-premise) đồng bộ liền mạch sang Amazon S3.
- Hệ thống tệp Windows hiện đại: Thiết lập thành công kiến trúc Amazon FSx có tính sẵn sàng cao, hỗ trợ các cơ chế chống trùng lặp dữ liệu nâng cao.
- Giám sát và bảo mật Pipeline: Sử dụng hiệu quả CloudWatch Events, KMS và X-Ray phục vụ cho công tác đo lường hiệu năng và tăng cường bảo mật cho pipeline.

