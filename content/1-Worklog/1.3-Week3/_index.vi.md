---
title: "Worklog Tuần 3"
date: 2026-07-08
weight: 1
chapter: false
pre: " <b> 1.3. </b> "
---


### Mục tiêu tuần 3
- Nâng cao hiệu suất quản trị thông qua giao diện dòng lệnh AWS CLI thay thế cho Console.
- Tìm hiểu về lưu trữ NoSQL với Amazon DynamoDB và tối ưu hóa tốc độ truy cập bằng Amazon ElastiCache.
- Thực hành các kiến thức mạng nâng cao như kết nối đa mạng và bảo mật hạ tầng.
- Triển khai giải pháp phân phối nội dung toàn cầu và xử lý dữ liệu tại biên mạng (Edge Computing).

### Bảng tiến độ theo ngày

| Ngày | Công việc chi tiết | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| Thứ 2 | AWS CLI: Tìm hiểu cấu hình Profile (Access Key/Secret Key), cài đặt môi trường và thực hành lệnh quản trị trên S3, EC2, IAM và VPC. | 04/05/2026 | 04/05/2026 | [AWS Documentation](https://000011.awsstudygroup.com/vi/) |
| Thứ 3 | Amazon DynamoDB: Tìm hiểu cấu trúc NoSQL (Table, Items, Attributes), Primary Key và Secondary Index. Thực hành CRUD bằng Console và Python SDK. | 05/05/2026 | 05/05/2026 | [AWS Documentation](https://000060.awsstudygroup.com/vi/) |
| Thứ 4 | Amazon ElastiCache: Thực hành tạo Cluster Redis (Mode enabled/disabled), tạo Subnet Group và sử dụng SDK để thực hiện các thao tác Set/Get dữ liệu. | 06/05/2026 | 06/05/2026 | [AWS Documentation](https://000061.awsstudygroup.com/vi/) |
| Thứ 5 | Networking Workshop: Thực hành Transit Gateway, Site-to-Site VPN để kết nối Hybrid Cloud. Cấu hình VPC Peering và thiết lập VPC Endpoints cho các dịch vụ AWS. | 07/05/2026 | 07/05/2026 | [AWS Documentation](https://000092.awsstudygroup.com/vi/) |
| Thứ 6 | Amazon CloudFront: Tạo Distribution, cấu hình các nguồn (S3/EC2 Origins), thiết lập Cache Behavior và tùy chỉnh trang báo lỗi (Custom Error Page). | 08/05/2026 | 08/05/2026 | [AWS Documentation](https://000094.awsstudygroup.com/vi/) |
| Thứ 7 | Edge Computing: Tìm hiểu kiến thức về Edge Locations. Thực hành tạo và triển khai hàm Lambda@Edge lên CloudFront để xử lý yêu cầu người dùng tại biên mạng. | 09/05/2026 | 10/05/2026 | [AWS Documentation](https://000130.awsstudygroup.com/vi/) |

### Kết quả đạt được tuần 3
- Quản trị bằng CLI: Có thể sử dụng AWS CLI để truy cập trực tiếp vào các API công khai của AWS, viết shell script để tự động hóa việc triển khai tài nguyên.
- Xử lý dữ liệu quy mô lớn: Hiểu cách vận hành DynamoDB cho các nhu cầu I/O cao và dữ liệu có cấu trúc không dự đoán được. Biết cách sử dụng ElastiCache để lưu trữ tạm thời nhanh cho lượng dữ liệu nhỏ, biến động cao.
- Thiết kế mạng phức tạp: Có khả năng thiết lập kết nối riêng tư giữa các VPC qua VPC Peering hoặc kết nối tập trung qua Transit Gateway. Hiểu cách sử dụng VPC Endpoints để kết nối dịch vụ mà không cần thông qua Internet công cộng.
- Phân phối và tăng tốc ứng dụng: Triển khai thành công Amazon CloudFront để lưu trữ tạm thời nội dung trên các máy chủ Edge, giúp giảm độ trễ và tăng tính sẵn sàng cho ứng dụng web.
- Lập trình tại biên: Bước đầu làm quen với việc triển khai logic xử lý ngay tại các trạm Edge thông qua Lambda@Edge, giúp tối ưu hóa phản hồi của khách hàng.

