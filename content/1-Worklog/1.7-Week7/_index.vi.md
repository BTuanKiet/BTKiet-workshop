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
| Thứ 2 | - Tìm hiểu AWS WAF Web ACLs và thực hành triển khai Managed Rules, Custom Rules để bảo vệ ứng dụng mẫu. | 01/06/2026 | 01/06/2026 | [AWS WAF](https://000026.awsstudygroup.com/vi/) |
| Thứ 3 | - Thực hành AWS KMS: mã hóa dữ liệu S3 Object, ghi nhật ký truy cập khóa qua CloudTrail và truy vấn bằng Athena. | 02/06/2026 | 02/06/2026 | [AWS KMS](https://000033.awsstudygroup.com/vi/) |
| Thứ 4 | - Kích hoạt Amazon Macie: quét S3 bucket phát hiện thông tin nhận dạng cá nhân (PII) và đánh giá bảo mật bucket.<br>- Thực hành AWS Secrets Manager: quản lý mật khẩu RDS, thiết lập tự động xoay vòng mật khẩu (Secret Rotation) và tích hợp với Fargate. | 03/06/2026 | 04/06/2026 | [Amazon Macie](https://000090.awsstudygroup.com/vi/)<br>[AWS Secrets Manager](https://000096.awsstudygroup.com/vi/) |
| Thứ 6 | - Thực hành AWS Firewall Manager: Audit và giới hạn Security Groups tập trung, thiết lập chính sách bảo mật cho SSH và RDP.<br>- Tìm hiểu AWS GuardDuty: cơ chế phát hiện mối đe dọa và xử lý các tình huống EC2 hoặc IAM credentials bị xâm nhập. | 05/06/2026 | 06/06/2026 | [AWS Firewall Manager](https://000097.awsstudygroup.com/vi/)<br>[AWS GuardDuty](https://000098.awsstudygroup.com/vi/) |

### Kết quả đạt được tuần 7
- Bảo vệ tầng ứng dụng: Sử dụng AWS WAF để lọc các request độc hại thông qua hệ thống managed rules và custom rules.
- An toàn dữ liệu lưu trữ: Hiểu rõ quy trình mã hóa dữ liệu với AWS KMS và kiểm soát toàn diện vết truy cập qua CloudTrail/Athena.
- Kiểm soát dữ liệu nhạy cảm: Sử dụng Amazon Macie tự động nhận diện dữ liệu PII và giám sát chặt chẽ các cấu hình bucket không an toàn.
- Quản lý định danh: Triển khai Secrets Manager giúp loại bỏ mật khẩu lưu dưới dạng văn bản thuần túy và thực hiện cơ chế xoay vòng tự động định kỳ.
- Quản trị mạng tập trung: Sử dụng Firewall Manager đảm bảo tính tuân thủ đồng bộ của Security Groups trên quy mô hệ thống lớn.
- Phát hiện xâm nhập: Vận hành Amazon GuardDuty phân tích hệ thống log giúp phát hiện nhanh các hành vi bất thường (đào tiền ảo, rò rỉ credentials).
