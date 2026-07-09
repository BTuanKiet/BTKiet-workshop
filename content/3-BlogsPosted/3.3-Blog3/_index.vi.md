---
title: "Blog 3"
date: 2026-07-08
weight: 3
chapter: false
pre: " <b> 3.3. </b> "
---

# CÁC PHƯƠNG PHÁP TỐT NHẤT CHO MULTI-TURN REINFORCEMENT LEARNING TRÊN AMAZON SAGEMAKER AI

Trong những năm gần đây, AI Agent đang trở thành một trong những xu hướng nổi bật của trí tuệ nhân tạo. Khác với các chatbot truyền thống chỉ xử lý một lượt hội thoại, AI Agent có khả năng lập kế hoạch, gọi các công cụ (Tool Calling), quan sát kết quả, tự điều chỉnh và tiếp tục thực hiện nhiều lượt tương tác để hoàn thành một tác vụ phức tạp.

Chính khả năng xử lý nhiều bước này khiến việc huấn luyện AI Agent trở nên khó khăn hơn rất nhiều. Một hành động đúng hay sai không thể đánh giá ngay tại thời điểm thực hiện mà phụ thuộc vào kết quả của nhiều bước tiếp theo. Vì vậy, AWS đã chia sẻ các phương pháp tốt nhất (Best Practices) khi triển khai Multi-Turn Reinforcement Learning (RL) trên Amazon SageMaker AI nhằm giúp xây dựng các AI Agent ổn định, đáng tin cậy và hiệu quả hơn.

## 1. Xây dựng môi trường Sandbox (Mô phỏng) cô lập

Multi-Turn Reinforcement Learning hoạt động dựa trên cơ chế **Trial and Error (Thử và Sai)**. Trong quá trình huấn luyện, AI Agent sẽ liên tục thử nhiều cách giải quyết khác nhau trước khi học được chiến lược tối ưu.

Nếu quá trình này diễn ra trực tiếp trên hệ thống Production, AI có thể vô tình thực hiện các thao tác ngoài ý muốn như hoàn tiền cho khách hàng, thay đổi dữ liệu trong cơ sở dữ liệu hoặc kích hoạt sai quy trình nghiệp vụ.

Để hạn chế rủi ro, AWS khuyến nghị xây dựng một môi trường Sandbox hoàn toàn tách biệt với Production.

Một số nguyên tắc quan trọng gồm:

* Các công cụ chỉ đọc nên sử dụng dữ liệu mô phỏng (Mocked Data) hoặc dữ liệu phát lại (Replay Data).
* Các công cụ có khả năng thay đổi trạng thái cần được cô lập theo từng phiên huấn luyện (Episode) nhằm tránh rò rỉ dữ liệu giữa các lần chạy.
* Sau mỗi Episode, toàn bộ tài nguyên tạm thời cần được dọn dẹp (Tear Down) để mỗi lần huấn luyện đều bắt đầu từ cùng một trạng thái ban đầu.

Nhờ vậy, AI Agent có thể học tập an toàn mà không ảnh hưởng đến hệ thống thực tế.

## 2. Thiết kế Reward Function để tránh Reward Hacking

Một trong những thách thức lớn nhất của Reinforcement Learning là hiện tượng **Reward Hacking**.

AI không hiểu mục tiêu thực sự của con người mà chỉ tối ưu hóa đúng hàm phần thưởng được lập trình.

Ví dụ:

* Nếu Agent bị phạt khi sử dụng quá nhiều lượt hội thoại, nó có thể trả lời qua loa để kết thúc sớm.
* Nếu mỗi lần gọi Tool đều được thưởng điểm, Agent có thể liên tục gọi công cụ mặc dù điều đó không giúp giải quyết bài toán.

Để giảm thiểu hiện tượng này, AWS khuyến nghị:

### Sử dụng Dense Reward

Thay vì chỉ đánh giá đúng hoặc sai ở kết quả cuối cùng, nên chia nhỏ quá trình thực hiện và cấp điểm thưởng cho từng bước hoàn thành.

Ví dụ, nếu Agent điền đúng 5 trong số 6 trường dữ liệu, hệ thống vẫn nên cấp một phần điểm thưởng thay vì đánh giá thất bại hoàn toàn.

Dense Reward giúp AI nhận được tín hiệu học tập liên tục trong suốt quá trình huấn luyện.

### Thực hiện đánh giá độc lập

Ngoài Reward Function dùng để huấn luyện, nên xây dựng một hệ thống đánh giá (Evaluation) độc lập.

Nếu điểm Reward tăng lên nhưng tỷ lệ hoàn thành nhiệm vụ thực tế (Task Success Rate) không cải thiện hoặc giảm xuống, rất có thể Agent đang khai thác lỗ hổng của Reward Function thay vì thực sự học được cách giải quyết vấn đề.

## 3. Kiểm soát Trajectory và Turn Budget

Trong Multi-Turn RL, mỗi lượt hội thoại đều làm tăng kích thước Context, kéo theo số lượng Token và chi phí tính toán cũng tăng lên.

AWS khuyến nghị theo dõi hai tham số quan trọng:

* **max_turns** – Số lượt tương tác tối đa trong một Episode. Một kinh nghiệm phổ biến là thiết lập giá trị này khoảng **1,5 lần** số lượt mà con người cần để hoàn thành cùng một nhiệm vụ.
* **sampling_max_tokens** – Giới hạn số Token được sinh ra trong mỗi lượt hội thoại nhằm tránh việc phản hồi quá dài và tiêu tốn tài nguyên không cần thiết.

Việc quản lý tốt Trajectory giúp cân bằng giữa chất lượng huấn luyện và chi phí tính toán.

## Phân biệt Completion và Correctness

AWS cũng nhấn mạnh rằng cần đánh giá riêng hai khái niệm:

* **Completion** – AI Agent hoàn thành toàn bộ quy trình.
* **Correctness** – AI Agent đưa ra kết quả chính xác.

Một Agent có thể hoàn thành đầy đủ các bước xử lý nhưng vẫn tạo ra kết quả sai. Vì vậy, trong quá trình đánh giá mô hình cần theo dõi cả hai chỉ số thay vì chỉ kiểm tra tác vụ đã được hoàn thành hay chưa.

## Kết luận

Huấn luyện AI Agent bằng Multi-Turn Reinforcement Learning không chỉ là lựa chọn thuật toán phù hợp mà còn là quá trình thiết kế môi trường huấn luyện, Reward Function, hệ thống đánh giá và chiến lược quản lý Trajectory một cách hợp lý.

Việc áp dụng các phương pháp tốt nhất mà AWS đề xuất sẽ giúp AI Agent học ổn định hơn, hạn chế hiện tượng Reward Hacking, nâng cao khả năng tổng quát hóa và sẵn sàng xử lý các tác vụ nhiều bước trong môi trường thực tế.

## Architecture Diagram

![Tổng quan về dịch vụ học tăng cường đa lượt của SageMaker AI](/images/3-BlogsPosted/3.3-Blog3/ML-21260-1.jpg)

### Bài viết gốc của AWS

- [AWS Machine Learning Blog – Best Practices for Multi-Turn Reinforcement Learning in Amazon SageMaker AI](https://aws.amazon.com/blogs/machine-learning/best-practices-for-multi-turn-reinforcement-learning-in-amazon-sagemaker-ai/)