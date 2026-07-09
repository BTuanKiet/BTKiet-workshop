---
title: "Worklog Tuần 8"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 1.8. </b> "
---


### Mục tiêu tuần 8
- Tự động hóa quy trình vá lỗi hệ điều hành (OS Patching) để giảm thiểu lỗ hổng bảo mật và gánh nặng vận hành.
- Triển khai giải pháp xác thực và quản lý danh tính tập trung cho ứng dụng web và di động.
- Áp dụng các tiêu chuẩn bảo mật khắt khe nhất để bảo vệ dữ liệu trên Amazon S3.
- Thiết lập cơ chế bảo vệ dữ liệu toàn diện và tự động hóa việc phục hồi tài nguyên.
- Nắm vững các phương thức kết nối mạng nâng cao, từ kết nối điểm - điểm đến mô hình quản trị mạng tập trung.

### Bảng tiến độ theo ngày

| Ngày | Công việc chi tiết | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| Thứ 2 | EC2 Image Builder: Thực hành xây dựng Pipeline tự động tạo AMI chứa các bản vá lỗi mới nhất theo phương pháp Blue/Green Deployment. | 08/06/2026 | 08/06/2026 | AWS Documentation |
| Thứ 3 | Amazon Cognito: Tìm hiểu User Pools và Identity Pools. Thực hành triển khai xác thực liên miền cho ứng dụng. | 09/06/2026 | 09/06/2026 | AWS Documentation |
| Thứ 4 | S3 Security Best Practices: Cấu hình chặn truy cập công khai (BPA), yêu cầu HTTPS/mã hóa SSE-S3 và sử dụng Access Analyzer. | 10/06/2026 | 10/06/2026 | AWS Documentation |
| Thứ 5 | AWS Backup: Khởi tạo Backup Plan để tự động sao lưu dữ liệu từ các dịch vụ EBS, RDS và DynamoDB. Thiết lập thông báo qua SNS. | 11/06/2026 | 11/06/2026 | AWS Documentation |
| Thứ 6 | VPC Peering: Thiết lập kết nối trực tiếp giữa các VPC bằng địa chỉ IP Private. Kích hoạt tính năng Cross-Peer DNS. | 12/06/2026 | 12/06/2026 | AWS Documentation |
| Thứ 7 - CN | AWS Transit Gateway: Triển khai mô hình Hub-and-Spoke kết nối đồng thời nhiều VPC thông qua một cổng quản trị tập trung duy nhất. | 13/06/2026 | 14/06/2026 | AWS Documentation |

### Kết quả đạt được tuần 8
- Vận hành hệ thống an toàn: Thành thạo sử dụng EC2 Image Builder kết hợp Systems Manager giúp tự động hóa đóng gói AMI và cập nhật bản vá.
- Làm chủ giải pháp định danh: Hiểu rõ cách sử dụng Cognito User Pools và Identity Pools trong việc quản lý và phân quyền truy cập cho khách hàng.
- Thắt chặt bảo mật lưu trữ: Áp dụng thành công rào bảo mật nâng cao cho S3 thông qua các chính sách AWS Config và VPC Endpoint Policy.
- Tự động hóa bảo vệ tài sản số: Triển khai giải pháp AWS Backup tập trung hóa toàn bộ quy trình sao lưu và đảm bảo khả năng phục hồi dữ liệu nhanh chóng.
- Tối ưu hóa hạ tầng mạng: Thiết lập thành công VPC Peering cho các kết nối đơn giản và triển khai AWS Transit Gateway để đơn giản hóa kiến trúc mạng đa VPC.

