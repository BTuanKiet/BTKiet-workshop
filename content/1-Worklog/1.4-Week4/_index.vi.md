---
title: "Worklog Tuần 4"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 1.4. </b> "
---


### Mục tiêu tuần 4
- Nắm vững kiến thức về điện toán không máy chủ (Serverless) và tự động hóa quy trình với AWS Lambda.
- Thành thạo kỹ năng tổ chức và quản lý tài nguyên quy mô lớn bằng Tags và Resource Groups.
- Nâng cao năng lực quản trị hệ thống tập trung và truy cập từ xa an toàn qua AWS Systems Manager.
- Tối ưu hóa hiệu suất và chi phí hạ tầng thông qua việc định cỡ tài nguyên (Right Sizing) chính xác.

### Bảng tiến độ theo ngày

| Ngày | Công việc chi tiết | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| Thứ 2 | AWS Lambda: Tìm hiểu kiến thức Serverless. Thực hành viết Function để tự động bật/tắt EC2 nhằm tối ưu chi phí. | 11/05/2026 | 11/05/2026 | AWS Documentation |
| Thứ 3 | Tags & Resource Groups: Gán nhãn metadata cho tài nguyên. Tạo nhóm tài nguyên dựa trên Tag để quản lý tập trung. | 12/05/2026 | 12/05/2026 | AWS Documentation |
| Thứ 4 | IAM Resource Tags: Thiết lập chính sách đặc quyền tối thiểu (Least Privilege) dựa trên Tag để kiểm soát quyền truy cập EC2. | 13/05/2026 | 13/05/2026 | AWS Documentation |
| Thứ 5 | AWS Systems Manager (SSM): Thực hành Patch Manager để vá lỗi đồng loạt và Run Command để chạy lệnh trên nhiều máy chủ. | 14/05/2026 | 14/05/2026 | AWS Documentation |
| Thứ 6 | SSM Session Manager: Thực hành kết nối từ xa vào máy chủ Linux/Windows (cả Public và Private) mà không cần mở cổng SSH/RDP. | 15/05/2026 | 15/05/2026 | AWS Documentation |
| Thứ 7 | EC2 Right Sizing: Sử dụng CloudWatch Agent thu thập dữ liệu bộ nhớ và dùng Compute Optimizer để lựa chọn cấu hình EC2 tối ưu. | 16/05/2026 | 17/05/2026 | AWS Documentation |

### Kết quả đạt được tuần 4
- Tự động hóa vận hành: Triển khai thành công AWS Lambda để thực hiện các tác vụ định kỳ như bật/tắt máy chủ theo lịch trình, giúp tiết kiệm chi phí cho các môi trường không cần chạy 24/7.
- Quản trị tài nguyên khoa học: Hiểu cách sử dụng Tag như một công cụ phân loại tài nguyên theo mục đích, chủ sở hữu hoặc môi trường (Dev/Test/Prod). Sử dụng Resource Groups để thực hiện các thao tác đồng loạt trên các nhóm tài nguyên này.
- Bảo mật phi tập trung: Áp dụng thành công việc phân quyền IAM dựa trên Resource Tags, chỉ cho phép người dùng thao tác với những tài nguyên có nhãn (Tag) tương ứng.
- Quản lý hệ thống hiện đại: Sử dụng AWS Systems Manager để tự động hóa việc bảo trì, vá lỗi phần mềm và thực thi lệnh từ xa mà không cần quản lý khóa SSH, tăng cường tính bảo mật cho hạ tầng.
- Tối ưu hóa tài nguyên: Biết cách sử dụng các chỉ số thực tế (CPU, RAM) thu thập từ máy chủ để đưa ra quyết định thay đổi loại Instance (Right Sizing) phù hợp, tránh lãng phí tài nguyên thừa.

