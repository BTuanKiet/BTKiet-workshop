---
title: "Worklog Tuần 8"
date: 2026-07-08
weight: 1
chapter: false
pre: " <b> 1.8. </b> "
---


### Mục tiêu tuần 8
- Tự động hóa quy trình vá lỗi hệ điều hành (OS Patching) để giảm thiểu lỗ hổng bảo mật và gánh nặng vận hành.
- Triển khai giải pháp xác thực và quản lý danh tính tập trung cho ứng dụng web và di động.
- Áp dụng các tiêu chuẩn bảo mật để bảo vệ dữ liệu trên Amazon S3.
- Thiết lập cơ chế bảo vệ dữ liệu toàn diện và tự động hóa việc phục hồi tài nguyên.
- Tìm hiểu các phương thức kết nối mạng nâng cao, từ kết nối điểm - điểm đến mô hình quản trị mạng tập trung.

### Bảng tiến độ theo ngày

| Ngày | Công việc chi tiết | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| Thứ 2 | - Thực hành EC2 Image Builder: xây dựng Pipeline tự động tạo AMI chứa các bản vá lỗi mới nhất theo phương pháp Blue/Green Deployment. | 08/06/2026 | 08/06/2026 | [EC2 Image Builder](https://000099.awsstudygroup.com/vi/) |
| Thứ 3 | - Tìm hiểu Amazon Cognito: User Pools và Identity Pools. Thực hành triển khai xác thực liên miền cho ứng dụng. | 09/06/2026 | 09/06/2026 | [Amazon Cognito](https://000141.awsstudygroup.com/vi/) |
| Thứ 4 | - Áp dụng S3 Security Best Practices: cấu hình chặn truy cập công khai (BPA), yêu cầu HTTPS/mã hóa SSE-S3 và dùng Access Analyzer.<br>- Khởi tạo AWS Backup Plan: tự động sao lưu dữ liệu từ EBS, RDS và DynamoDB, thiết lập thông báo qua SNS. | 10/06/2026 | 11/06/2026 | [S3 Security](https://000069.awsstudygroup.com/vi/)<br>[AWS Backup](https://000013.awsstudygroup.com/vi/) |
| Thứ 6 | - Thiết lập VPC Peering: kết nối trực tiếp giữa các VPC bằng địa chỉ IP Private, kích hoạt tính năng Cross-Peer DNS.<br>- Triển khai AWS Transit Gateway theo mô hình Hub-and-Spoke: kết nối đồng thời nhiều VPC thông qua một cổng quản trị tập trung. | 12/06/2026 | 13/06/2026 | [VPC Peering](https://000019.awsstudygroup.com/vi/)<br>[AWS Transit Gateway](https://000020.awsstudygroup.com/vi/) |

### Kết quả đạt được tuần 8
- Vận hành hệ thống an toàn: Biết cách sử dụng EC2 Image Builder kết hợp Systems Manager để tự động hóa đóng gói AMI và cập nhật bản vá.
- Giải pháp định danh: Hiểu rõ cách sử dụng Cognito User Pools và Identity Pools trong việc quản lý và phân quyền truy cập cho khách hàng.
- Thắt chặt bảo mật lưu trữ: Áp dụng thành công rào bảo mật nâng cao cho S3 thông qua các chính sách AWS Config và VPC Endpoint Policy.
- Tự động hóa bảo vệ tài sản số: Triển khai giải pháp AWS Backup tập trung hóa toàn bộ quy trình sao lưu và đảm bảo khả năng phục hồi dữ liệu nhanh chóng.
- Tối ưu hóa hạ tầng mạng: Thiết lập thành công VPC Peering cho các kết nối đơn giản và triển khai AWS Transit Gateway để đơn giản hóa kiến trúc mạng đa VPC.
