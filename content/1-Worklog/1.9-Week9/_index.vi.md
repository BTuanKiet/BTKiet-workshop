---
title: "Worklog Tuần 9"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 1.9. </b> "
---


### Mục tiêu tuần 9
- Nắm vững kiến thức và thực hành xây dựng kiến trúc hướng sự kiện (Event-driven) giúp các dịch vụ hoạt động độc lập.
- Triển khai giải pháp lưu trữ chia sẻ hiệu năng cao với Amazon EBS Multi-Attach.
- Xây dựng hệ thống có tính sẵn sàng cao (High Availability) cho MySQL và Windows Failover Cluster.
- Làm chủ công nghệ Container (Docker) và điều phối qua hệ thống Amazon ECS.

### Bảng tiến độ theo ngày

| Ngày | Công việc chi tiết | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| Thứ 2 | SQS & SNS: Xây dựng kiến trúc Pub/Sub. Thực hành lọc tin nhắn (Message Filtering) để điều hướng trực tiếp đến các subscriber cụ thể. | 15/06/2026 | 15/06/2026 | AWS Documentation |
| Thứ 3 | EBS Multi-Attach: Kết nối đồng thời nhiều máy chủ EC2 vào một ổ đĩa EBS. Sử dụng hệ thống NVMe Reservation để điều phối quyền truy cập. | 16/06/2026 | 16/06/2026 | AWS Documentation |
| Thứ 4 | MySQL HA Cluster: Triển khai MySQL Cluster dùng Multi-Attach và LVM. Tự động hóa quy trình khắc phục sự cố qua Systems Manager Runbook. | 17/06/2026 | 17/06/2026 | AWS Documentation |
| Thứ 5 | Windows Failover Cluster: Thiết lập cụm máy chủ chịu lỗi, tiến hành cấu hình thành phần Cluster Quorum và Shared Storage. | 18/06/2026 | 18/06/2026 | AWS Documentation |
| Thứ 6 | Docker: Đóng gói ứng dụng mẫu, sử dụng công cụ Docker Compose quản lý các container và đẩy image hoàn chỉnh lên Amazon ECR. | 19/06/2026 | 19/06/2026 | AWS Documentation |
| Thứ 7 - CN | Amazon ECS: Khởi tạo Cluster, Task Definitions, thực hiện cấu hình Application Load Balancer và triển khai mô hình Blue/Green. | 20/06/2026 | 21/06/2026 | AWS Documentation |

### Kết quả đạt được tuần 9
- Xây dựng hệ thống linh hoạt: Hiểu cách phối hợp bộ đôi SNS và SQS để xếp hàng tin nhắn, giảm tải triệt để các logic định tuyến trực tiếp phức tạp.
- Làm chủ lưu trữ dùng chung: Triển khai thành công giải pháp chia sẻ dữ liệu an toàn giữa các máy chủ Linux/Windows bằng ổ đĩa EBS Multi-Attach.
- Thiết kế hệ thống chịu lỗi: Vận hành tốt hệ thống Windows Failover Cluster và thực hiện kết hợp Internal NLB đồng bộ với cụm MySQL Cluster.
- Hiện đại hóa với Container: Thành thạo quy trình đóng gói mã nguồn bằng Docker và quản lý, điều phối container linh hoạt qua dịch vụ Amazon ECS.
- Vận hành tự động: Sử dụng hiệu quả Systems Manager và Incident Manager để thiết lập kịch bản ứng phó, giảm thiểu thời gian xử lý sự cố hệ thống (MTTR).

