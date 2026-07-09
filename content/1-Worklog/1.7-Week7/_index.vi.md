---
title: "Worklog Tuần 7"
date: 2026-07-08
weight: 1
chapter: false
pre: " <b> 1.7. </b> "
---


### Mục tiêu tuần 7
- Thiết lập các lớp phòng thủ ứng dụng web để ngăn chặn các cuộc tấn công khai thác lỗ hổng phổ biến.
- Triển khai giải pháp mã hóa dữ liệu và quản lý thông tin xác thực (credentials) tập trung, an toàn.
- Tự động hóa quy trình phát hiện dữ liệu nhạy cảm và giám sát các mối đe dọa an ninh trên toàn hệ thống.
- Hệ thống hóa việc quản trị chính sách tường lửa mạng cho quy mô nhiều tài khoản.

### Bảng tiến độ theo ngày

| Ngày | Công việc chi tiết | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| Thứ 2 | AWS Web Application Firewall (WAF): Tìm hiểu Web ACLs. Thực hành triển khai Managed Rules và Custom Rules bảo vệ ứng dụng mẫu. | 01/06/2026 | 01/06/2026 | AWS Documentation |
| Thứ 3 | AWS Key Management Service (KMS): Thực hành mã hóa dữ liệu S3 Object. Ghi nhật ký truy cập khóa qua CloudTrail và truy vấn bằng Athena. | 02/06/2026 | 02/06/2026 | AWS Documentation |
| Thứ 4 | Amazon Macie: Kích hoạt dịch vụ, thực hiện quét S3 bucket phát hiện thông tin nhận dạng cá nhân (PII) và đánh giá bảo mật bucket. | 03/06/2026 | 03/06/2026 | AWS Documentation |
| Thứ 5 | AWS Secrets Manager: Quản lý mật khẩu RDS. Thiết lập tự động xoay vòng mật khẩu (Secret Rotation) và tích hợp với Fargate. | 04/06/2026 | 04/06/2026 | AWS Documentation |
| Thứ 6 | AWS Firewall Manager: Thực hiện Audit và giới hạn Security Groups tập trung. Thiết lập chính sách bảo mật cho SSH và RDP. | 05/06/2026 | 05/06/2026 | AWS Documentation |
| Thứ 7 | AWS GuardDuty: Tìm hiểu cơ chế phát hiện mối đe dọa. Thực hành xử lý các tình huống EC2 hoặc IAM credentials bị xâm nhập. | 06/06/2026 | 07/06/2026 | AWS Documentation |

### Kết quả đạt được tuần 7
- Bảo vệ tầng ứng dụng: Làm chủ AWS WAF nhằm lọc các request độc hại thông qua hệ thống managed rules và custom rules.
- An toàn dữ liệu lưu trữ: Nắm vững quy trình mã hóa dữ liệu với AWS KMS và kiểm soát toàn diện vết truy cập qua CloudTrail/Athena.
- Kiểm soát dữ liệu nhạy cảm: Sử dụng Amazon Macie tự động nhận diện dữ liệu PII và giám sát chặt chẽ các cấu hình bucket không an toàn.
- Quản lý định danh thông minh: Triển khai Secrets Manager giúp loại bỏ mật khẩu lưu dưới dạng văn bản thuần túy và thực hiện cơ chế xoay vòng tự động định kỳ.
- Quản trị mạng tập trung: Sử dụng Firewall Manager đảm bảo tính tuân thủ đồng bộ của Security Groups trên quy mô hệ thống lớn.
- Phát hiện xâm nhập chủ động: Vận hành Amazon GuardDuty phân tích hệ thống log giúp phát hiện nhanh các hành vi bất thường (đào tiền ảo, rò rỉ credentials).

