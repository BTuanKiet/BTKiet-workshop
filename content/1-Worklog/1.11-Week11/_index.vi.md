---
title: "Worklog Tuần 11"
date: 2026-07-08
weight: 2
chapter: false
pre: " <b> 1.11. </b> "
---


### Mục tiêu tuần 11
- Tìm hiểu các mẫu thiết kế nâng cao và xây dựng các kiến trúc ứng dụng quy mô toàn cầu với Amazon DynamoDB.
- Học kỹ năng điều phối các quy trình công việc phức tạp giữa các dịch vụ thông qua AWS Step Functions.
- Thực hành tối ưu hóa hiệu suất hạ tầng lưu trữ đối với Amazon S3 và Amazon EFS.
- Triển khai chiến lược tối ưu hóa chi phí dài hạn qua mô hình Savings Plans và Reserved Instances.
- Xây dựng nền tảng phân tích chuyên sâu các dữ liệu liên quan đến chi phí bằng bộ đôi AWS Glue và Amazon Athena.

### Bảng tiến độ theo ngày

| Ngày | Công việc chi tiết | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| Thứ 2 | - Thực hành Xây dựng ứng dụng nâng cao với Amazon DynamoDB. | 29/06/2026 | 29/06/2026 | [Amazon DynamoDB](https://000039.awsstudygroup.com/vi/) |
| Thứ 3 | - Thực hành AWS Step Functions: điều phối các workflow, làm việc với các trạng thái xử lý và thiết lập cơ chế xử lý lỗi (Retry/Catch). | 30/06/2026 | 30/06/2026 | [AWS Step Functions](https://000047.awsstudygroup.com/vi/) |
| Thứ 4 | - Thực hành Storage Performance Workshop: tối ưu thông lượng khi thao tác với tệp nhỏ trên S3 và cấu hình hiệu suất nâng cao cho Amazon EFS. | 01/07/2026 | 01/07/2026 | [Storage Performance Workshop](https://000068.awsstudygroup.com/vi/) |
| Thứ 5 | - Phân tích và áp dụng Savings Plans: mua và áp dụng các gói tiết kiệm chi phí cho EC2 và RDS.<br>- Trực quan hóa chi phí: giám sát theo tài khoản, kiểm tra tỷ lệ bao phủ SP và RI, kết xuất báo cáo EC2 tùy chỉnh. | 02/07/2026 | 03/07/2026 | [Savings Plans](https://000042.awsstudygroup.com/vi/)<br>[Cost Visualization](https://000034.awsstudygroup.com/vi/) |
| Thứ 6 | - Xây dựng nền tảng phân tích chi phí nâng cao với AWS Glue và Amazon Athena để thu thập và phân tích báo cáo chi phí chuyên sâu. | 04/07/2026 | 04/07/2026 | [AWS Glue & Amazon Athena](https://000040.awsstudygroup.com/vi/) |

### Kết quả đạt được tuần 11
- DynamoDB quy mô lớn: Áp dụng các mẫu thiết kế nâng cao (Sparse GSI, Composite Keys) và có khả năng cấu hình Global Tables đạt độ trễ mili giây.
- Điều phối Workflow: Xây dựng thành công các State Machines phức tạp tích hợp khả năng xử lý các tác vụ song song đồng thời thông qua Step Functions.
- Tối ưu hóa hạ tầng lưu trữ: Áp dụng được kỹ thuật song song hóa luồng đọc/ghi trên Amazon S3 (giúp hệ thống đạt ngưỡng 55.000 request/s) và cấu hình tối ưu EFS đạt mức thông lượng trên 10 GiBps.
- Quản trị tài chính: Áp dụng hiệu quả các mô hình Savings Plans và Reserved Instances tuân thủ theo đúng bộ khung chuẩn AWS Well-Architected Framework.
- Phân tích dữ liệu chi phí: Xây dựng nền tảng tự động phân tích báo cáo chi phí chi tiết (CUR), giúp tổ chức truy vấn linh hoạt phục vụ công tác tối ưu hóa hạ tầng.
