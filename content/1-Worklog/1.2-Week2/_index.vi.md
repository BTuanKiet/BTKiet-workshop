---
title: "Worklog Tuần 2"
date: 2026-07-08
weight: 1
chapter: false
pre: " <b> 1.2. </b> "
---


### Mục tiêu tuần 2
- Tìm hiểu về dịch vụ cơ sở dữ liệu quan hệ (RDS) và các chiến lược sẵn sàng cao (High Availability).
- Thực hành triển khai ứng dụng và container một cách đơn giản, tối ưu chi phí thông qua Amazon Lightsail.
- Hiểu và vận hành cơ chế tự động mở rộng hệ thống (Auto Scaling) và cân bằng tải.
- Thiết lập hệ thống giám sát tập trung và quản lý tên miền cho ứng dụng.

### Bảng tiến độ theo ngày

| Ngày | Công việc chi tiết | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| Thứ 2 | Amazon RDS: Tìm hiểu về OLTP, các loại DB engine hỗ trợ và tính năng quản lý tự động (backup, patching). | 27/04/2026 | 27/04/2026 | [AWS Documentation](https://000005.awsstudygroup.com/vi/) |
| Thứ 3 | Amazon Lightsail: Thực hành triển khai WordPress, PrestaShop và tối ưu hóa chi phí trên Lightsail. | 28/04/2026 | 28/04/2026 | [AWS Documentation](https://000045.awsstudygroup.com/vi/) |
| Thứ 4 | Lightsail Containers: Tìm hiểu cách xây dựng và triển khai ứng dụng container hóa từ Docker image lên Lightsail. | 29/04/2026 | 29/04/2026 | [AWS Documentation](https://000046.awsstudygroup.com/vi/) |
| Thứ 5 | EC2 Auto Scaling: Thiết lập Launch Template, Load Balancer và cấu hình Auto Scaling Group (ASG) cho ứng dụng FCJ Management. | 30/04/2026 | 30/04/2026 | [AWS Documentation](https://000006.awsstudygroup.com/vi/) |
| Thứ 6 | Amazon CloudWatch: Cấu hình giám sát Metrics, Logs và thiết lập Alarms để cảnh báo sự cố hệ thống. | 01/05/2026 | 01/05/2026 | [AWS Documentation](https://000008.awsstudygroup.com/vi/) |
| Thứ 7 | Amazon Route 53: Tìm hiểu về DNS và thực hành thiết lập kiến trúc Hybrid DNS với Route 53 Resolver. | 02/05/2026 | 03/05/2026 | [AWS Documentation](https://000010.awsstudygroup.com/vi/) |

### Kết quả đạt được tuần 2
- Hiểu về cơ sở dữ liệu: Biết cách triển khai Amazon RDS với cấu hình Multi-AZ để đảm bảo tính sẵn có cao và sử dụng Read Replicas để mở rộng khả năng đọc.
- Triển khai ứng dụng nhanh chóng: Biết cách sử dụng Amazon Lightsail để host các ứng dụng mã nguồn mở và quản lý ứng dụng container hóa mà không cần lo lắng về quản lý hạ tầng phức tạp.
- Tự động hóa hạ tầng: Triển khai thành công hệ thống có khả năng tự phục hồi và tự động co giãn theo lưu lượng truy cập thực tế bằng cách kết hợp ASG và Elastic Load Balancing.
- Giám sát toàn diện: Có khả năng thu thập và phân tích dữ liệu hiệu năng (Metrics/Logs) thông qua CloudWatch, giúp giảm thời gian giải quyết sự cố (MTTR).
- Quản trị DNS: Hiểu cách cấu hình Route 53 Resolver (Inbound/Outbound Endpoints) để tích hợp hệ thống DNS on-premise với AWS.

