---
title: "Blog 2"
date: 2026-07-08
weight: 2
chapter: false
pre: " <b> 3.2. </b> "
---

# TRIỂN KHAI HẠ TẦNG MULTI-TURN RL CHO AMAZON NOVA TRÊN AMAZON SAGEMAKER HYPERPOD

Khi các AI Agent ngày càng phải xử lý các tác vụ nhiều bước, các phương pháp Reinforcement Learning truyền thống không còn đáp ứng tốt yêu cầu huấn luyện. AWS đã giới thiệu một kiến trúc tham khảo giúp triển khai hạ tầng **Multi-Turn Reinforcement Learning (RL)** sử dụng **Amazon SageMaker HyperPod** và **Amazon Nova** nhằm huấn luyện các AI Agent có khả năng đưa ra quyết định qua nhiều lượt tương tác.

## Tại sao cần Multi-Turn RL?

Các phương pháp như RLHF chủ yếu tối ưu từng phản hồi riêng lẻ. Tuy nhiên, trong thực tế, AI Agent thường phải thực hiện nhiều bước liên tiếp như:

- Gọi API
- Truy vấn cơ sở dữ liệu
- Xử lý lỗi hệ thống
- Sử dụng nhiều công cụ trong cùng một phiên làm việc

Chất lượng của mỗi hành động phụ thuộc vào kết quả của các bước tiếp theo, vì vậy Multi-Turn RL trở thành phương pháp phù hợp hơn.

## Kiến trúc chính

Giải pháp được xây dựng theo mô hình kiến trúc hướng sự kiện (Event-driven Architecture) gồm ba thành phần chính.

### Amazon SageMaker HyperPod

Amazon SageMaker HyperPod cung cấp các cụm GPU hiệu năng cao (P5 instances) để chạy quá trình suy luận và huấn luyện mô hình. Trong quá trình này, mô hình được tối ưu bằng thuật toán **Group Relative Policy Optimization (GRPO)** dựa trên tín hiệu phần thưởng từ môi trường đánh giá.

### AWS Fargate (Amazon ECS)

Amazon ECS chạy trên AWS Fargate triển khai **Reward Environment**. Trong bài minh họa của AWS, môi trường đánh giá là trò chơi Wordle, có nhiệm vụ chấm điểm phản hồi của mô hình và trả về tín hiệu phần thưởng.

### Amazon Nova Forge SDK

Amazon Nova Forge SDK đóng vai trò điều phối việc trao đổi thông tin giữa mô hình và môi trường đánh giá, đồng thời quản lý trạng thái hội thoại trong suốt quá trình tương tác nhiều lượt.

## Mô hình triển khai hai giai đoạn

Hệ thống được triển khai theo hai giai đoạn riêng biệt.

### Giai đoạn 1 – Triển khai hạ tầng

Sử dụng AWS CDK để triển khai các thành phần hạ tầng như:

- Amazon VPC
- Amazon EKS
- Amazon ECS
- Amazon S3
- AWS Step Functions

Quá trình này chỉ cần thực hiện một lần.

### Giai đoạn 2 – Chạy huấn luyện

Khi tập dữ liệu định dạng `.jsonl` được tải lên Amazon S3, quy trình huấn luyện sẽ tự động được kích hoạt.

AWS Step Functions sẽ khởi tạo các tài nguyên cần thiết trên HyperPod, chạy quá trình huấn luyện, điều phối môi trường đánh giá và tự động giải phóng tài nguyên sau khi hoàn thành nhằm tối ưu chi phí sử dụng GPU.

## Lợi ích

Kiến trúc này mang lại nhiều lợi ích như:

- Tự động hóa toàn bộ quy trình huấn luyện
- Kiến trúc hướng sự kiện giúp dễ mở rộng
- Tối ưu việc sử dụng GPU
- Giảm chi phí vận hành
- Dễ dàng theo dõi tiến trình thông qua AWS Step Functions
- Phù hợp cho các hệ thống Reinforcement Learning quy mô lớn

## Kiến trúc hệ thống

![Hạ tầng Multi-Turn RL cho Amazon Nova trên SageMaker HyperPod](/images/3-BlogsPosted/3.2-Blog2/ML-20450-1.png)

## Tài liệu tham khảo

- AWS Machine Learning Blog:
  https://aws.amazon.com/blogs/machine-learning/deploying-multi-turn-rl-infrastructure-for-amazon-nova-on-amazon-sagemaker-hyperpod/
