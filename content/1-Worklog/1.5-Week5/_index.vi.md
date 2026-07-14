---
title: "Worklog Tuần 5"
date: 2026-07-08
weight: 1
chapter: false
pre: " <b> 1.5. </b> "
---


### Mục tiêu tuần 5
- Tăng cường khả năng giám sát và bảo mật hạ tầng mạng thông qua phân tích lưu lượng.
- Tìm hiểu các kỹ năng quản trị tài chính, ủy quyền thanh toán và quản lý hạn ngạch dịch vụ trên AWS.
- Thực hiện các phương pháp tốt nhất (Best Practices) trong việc kiểm soát chi phí sử dụng tài nguyên.
- Tự động hóa quy trình bảo vệ dữ liệu và triển khai hệ thống phát hiện bất thường cho các bản sao lưu.

### Bảng tiến độ theo ngày

| Ngày | Công việc chi tiết | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| Thứ 2 | VPC Flow Logs: Tìm hiểu và kích hoạt nhật ký lưu lượng IP. Thực hành xuất dữ liệu sang CloudWatch Logs để chẩn đoán quy tắc Security Group. | 18/05/2026 | 18/05/2026 | [AWS Documentation](https://000074.awsstudygroup.com/vi/) |
| Thứ 3 | Ủy quyền Billing: Thực hành từ tài khoản Root để cấp quyền truy cập bảng điều khiển thanh toán cho IAM User/Group. | 19/05/2026 | 19/05/2026 | [AWS Documentation](https://000075.awsstudygroup.com/vi/) |
| Thứ 4 | Service Quotas: Tìm hiểu cách tra cứu mức giới hạn dịch vụ và quy trình gửi yêu cầu tăng hạn ngạch tài nguyên cho AWS Support. | 20/05/2026 | 20/05/2026 | [AWS Documentation](https://000063.awsstudygroup.com/vi/) |
| Thứ 5 | Quản trị chi phí với IAM: Thực hành thiết lập các Policy giới hạn sử dụng theo Region, loại máy chủ (EC2 Family) và kích thước ổ đĩa (EBS). | 21/05/2026 | 21/05/2026 | [AWS Documentation](https://000064.awsstudygroup.com/vi/) |
| Thứ 6 | Data Lifecycle Manager (DLM): Thiết lập các chính sách tự động hóa việc tạo, lưu trữ và xóa snapshot cho hệ thống ổ đĩa EBS. | 22/05/2026 | 22/05/2026 | [AWS Documentation](https://000088.awsstudygroup.com/vi/) |
| Thứ 7 | EBS Backup Anomaly Detection: Xây dựng pipeline serverless (Lambda, EventBridge, CloudWatch) để phát hiện thay đổi bất thường giữa các bản snapshot. | 23/05/2026 | 24/05/2026 | [AWS Documentation](https://000089.awsstudygroup.com/vi/) |

### Kết quả đạt được tuần 5
- Minh bạch hóa lưu lượng mạng: Hiểu cách sử dụng VPC Flow Logs để theo dõi lưu lượng đến/đi mà không ảnh hưởng đến hiệu suất mạng, giúp xác định các hướng truy cập trái phép hoặc cấu hình sai tường lửa.
- Quản trị tài chính an toàn: Biết cách phân tách quyền hạn, cho phép nhân viên kế toán hoặc quản lý truy cập dữ liệu Billing mà không cần chia sẻ thông tin đăng nhập của tài khoản Root.
- Quản lý tài nguyên có kế hoạch: Biết cách sử dụng Service Quotas để lập kế hoạch mở rộng hạ tầng, tránh tình trạng bị gián đoạn do chạm ngưỡng giới hạn mặc định của AWS.
- Tối ưu hóa chi phí: Áp dụng thành công các Policy IAM để ngăn chặn việc khởi tạo tài nguyên lãng phí.
- Bảo vệ dữ liệu tự động: Triển khai giải pháp Amazon DLM giúp duy trì tính tuân thủ sao lưu dài hạn (trên 90 ngày) với chi phí tối ưu qua Snapshot Archive.
- Phát hiện đe dọa sớm: Thiết lập cơ chế phát hiện bất thường của CloudWatch, phát cảnh báo qua SNS khi có sự thay đổi đột biến trong dữ liệu bản sao lưu.

