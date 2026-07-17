---
title: "Worklog Tuần 6"
date: 2026-07-08
weight: 1
chapter: false
pre: " <b> 1.6. </b> "
---


### Mục tiêu tuần 6
- Tối ưu hóa quy trình phát triển và lập trình trên đám mây bằng các công cụ hỗ trợ AI thông minh.
- Thiết lập các "hàng rào bảo vệ" nâng cao cho quyền hạn người dùng để ngăn chặn rò rỉ hoặc leo thang đặc quyền.
- Tìm hiểu cơ chế kiểm soát truy cập dựa trên ngữ cảnh thực tế (thời gian, địa chỉ IP, nhãn tài nguyên).
- Triển khai hệ thống giám sát tuân thủ bảo mật tập trung cho toàn bộ hạ tầng AWS.
- Đảm bảo an toàn dữ liệu bằng cách thiết lập các đường kết nối riêng tư giữa hạ tầng mạng và lưu trữ S3.

### Bảng tiến độ theo ngày

| Ngày | Công việc chi tiết | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| Thứ 2 | - Cài đặt AWS Toolkit for VS Code, kết nối tài khoản AWS và thực hành dùng Amazon Q, CodeWhisperer để gợi ý mã nguồn. | 25/05/2026 | 25/05/2026 | [AWS Toolkit for VS Code](https://000087.awsstudygroup.com/vi/) |
| Thứ 3 | - Tìm hiểu IAM Permission Boundaries: lý thuyết về giới hạn quyền tối đa và thực hành tạo Policy giới hạn để đóng lỗ hổng privilege escalation. | 26/05/2026 | 26/05/2026 | [IAM Permission Boundaries](https://000030.awsstudygroup.com/vi/) |
| Thứ 4 | - Thiết lập IAM Policies & Conditions: điều kiện bảo mật bổ sung như giới hạn theo IP nguồn, giờ làm việc và Resource Tags.<br>- Kích hoạt AWS Security Hub: kiểm tra tự động các tiêu chuẩn bảo mật và quản lý cảnh báo tập trung. | 27/05/2026 | 28/05/2026 | [IAM Policies & Conditions](https://000044.awsstudygroup.com/vi/)<br>[AWS Security Hub](https://000018.awsstudygroup.com/vi/) |
| Thứ 6 | - Cấu hình VPC Endpoints cho S3: Gateway Endpoint và Interface Endpoint để truy cập S3 thông qua AWS PrivateLink. | 29/05/2026 | 29/05/2026 | [VPC Endpoints](https://000111.awsstudygroup.com/vi/) |

### Kết quả đạt được tuần 6
- Nâng cao hiệu suất lập trình: Biết cách sử dụng các trợ lý AI trong VS Code để viết unit test, gỡ lỗi và tối ưu hóa mã nguồn.
- Kiểm soát quyền hạn chặt chẽ: Hiểu rõ sự khác biệt giữa quyền hạn được gán và quyền hạn thực tế thông qua Permission Boundaries.
- Bảo mật theo ngữ cảnh: Có khả năng thiết lập các rào cản bảo mật đa lớp dựa trên các điều kiện thực tế (IP văn phòng, giờ hành chính).
- Quản trị bảo mật tập trung: Triển khai Dashboard của Security Hub theo dõi trạng thái tuân thủ bảo mật theo thời gian thực.
- Thiết lập kết nối an toàn Hybrid: Triển khai thành công AWS PrivateLink, cho phép truy xuất dữ liệu an toàn trên S3 mà không cần đi qua Internet công cộng.
