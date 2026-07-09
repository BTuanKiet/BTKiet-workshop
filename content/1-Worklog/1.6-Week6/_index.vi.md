---
title: "Worklog Tuần 6"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 1.6. </b> "
---


### Mục tiêu tuần 6
- Tối ưu hóa quy trình phát triển và lập trình trên đám mây bằng các công cụ hỗ trợ AI thông minh.
- Thiết lập các "hàng rào bảo vệ" nâng cao cho quyền hạn người dùng để ngăn chặn rò rỉ hoặc leo thang đặc quyền.
- Nắm vững cơ chế kiểm soát truy cập dựa trên ngữ cảnh thực tế (thời gian, địa chỉ IP, nhãn tài nguyên).
- Triển khai hệ thống giám sát tuân thủ bảo mật tập trung cho toàn bộ hạ tầng AWS.
- Đảm bảo an toàn dữ liệu tuyệt đối bằng cách thiết lập các đường kết nối riêng tư giữa hạ tầng mạng và lưu trữ S3.

### Bảng tiến độ theo ngày

| Ngày | Công việc chi tiết | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| Thứ 2 | AWS Toolkit for VS Code: Cài đặt tiện ích, kết nối tài khoản AWS. Thực hành sử dụng Amazon Q và CodeWhisperer để gợi ý mã nguồn. | 25/05/2026 | 25/05/2026 | AWS Documentation |
| Thứ 3 | IAM Permission Boundaries: Tìm hiểu lý thuyết về giới hạn quyền tối đa. Thực hành tạo Policy giới hạn để đóng lỗ hổng privilege escalation. | 26/05/2026 | 26/05/2026 | AWS Documentation |
| Thứ 4 | IAM Policies & Conditions: Thiết lập điều kiện bảo mật bổ sung như giới hạn theo IP nguồn, giờ làm việc và Resource Tags. | 27/05/2026 | 27/05/2026 | AWS Documentation |
| Thứ 5 | AWS Security Hub: Kích hoạt dịch vụ, thực hiện kiểm tra tự động các tiêu chuẩn bảo mật và quản lý cảnh báo tập trung. | 28/05/2026 | 28/05/2026 | AWS Documentation |
| Thứ 6 | VPC Endpoints cho S3: Cấu hình Gateway Endpoint và Interface Endpoint để truy cập S3 thông qua giải pháp AWS PrivateLink. | 29/05/2026 | 31/05/2026 | AWS Documentation |

### Kết quả đạt được tuần 6
- Nâng cao hiệu suất lập trình: Thành thạo sử dụng các trợ lý AI trong VS Code để viết unit test, gỡ lỗi và tối ưu hóa mã nguồn.
- Kiểm soát quyền hạn chặt chẽ: Hiểu rõ sự khác biệt giữa quyền hạn được gán và quyền hạn thực tế thông qua Permission Boundaries.
- Bảo mật theo ngữ cảnh: Có khả năng thiết lập các rào cản bảo mật đa lớp dựa trên các điều kiện thực tế (IP văn phòng, giờ hành chính).
- Quản trị bảo mật tập trung: Triển khai Dashboard của Security Hub theo dõi trạng thái tuân thủ bảo mật theo thời gian thực.
- Thiết lập kết nối an toàn Hybrid: Triển khai thành công AWS PrivateLink, cho phép truy xuất dữ liệu an toàn trên S3 mà không cần đi qua Internet công cộng.

