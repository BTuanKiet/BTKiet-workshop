---
title: "Blog 1"
date: 2026-07-08
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---

# BETTER TOGETHER: AMAZON EKS AUTO MODE VÀ ISTIO AMBIENT MESH

Khi kiến trúc microservices ngày càng phát triển với hàng chục hoặc hàng trăm dịch vụ, việc vận hành Kubernetes trở nên phức tạp hơn. Hai thách thức phổ biến là quản lý hạ tầng tính toán và bảo mật giao tiếp giữa các dịch vụ.

AWS giới thiệu sự kết hợp giữa **Amazon EKS Auto Mode** và **Istio Ambient Mesh** nhằm giúp doanh nghiệp tự động hóa việc quản lý hạ tầng Kubernetes, đồng thời tăng cường bảo mật và tối ưu hóa việc giao tiếp giữa các microservices.

## Amazon EKS Auto Mode

Amazon EKS Auto Mode giúp đơn giản hóa việc quản lý cụm Kubernetes bằng cách tự động vận hành hạ tầng tính toán.

Các tính năng nổi bật bao gồm:

- Tự động provisioning, scaling, patching và cập nhật các node.
- Sử dụng hệ điều hành Bottlerocket được tối ưu cho container.
- AWS quản lý sẵn các thành phần như Amazon VPC CNI, kube-proxy, CoreDNS và Amazon EBS CSI Driver.
- Tự động lựa chọn loại EC2 phù hợp và tối ưu chi phí thông qua Karpenter.

Nhờ đó, người dùng có thể tập trung phát triển ứng dụng thay vì quản lý hạ tầng Kubernetes.

## Istio Ambient Mesh

Istio Ambient Mesh mang đến kiến trúc service mesh không sử dụng sidecar (sidecarless), giúp giảm mức tiêu thụ tài nguyên trong khi vẫn đảm bảo giao tiếp an toàn giữa các dịch vụ.

Kiến trúc gồm ba thành phần chính.

### ztunnel

ztunnel là proxy Layer 3/4 chạy trên mỗi node, cung cấp:

- Mã hóa mTLS
- Định danh dịch vụ
- Chính sách bảo mật Layer 4
- Thu thập TCP Telemetry

### Waypoint Proxy

Waypoint Proxy là proxy Layer 7 được triển khai khi cần các tính năng nâng cao như:

- HTTP Routing
- Traffic Management
- Retries
- Circuit Breaking
- Chính sách phân quyền HTTP

### Giao thức HBONE

HBONE (HTTP-Based Overlay Network Environment) là giao thức giúp truyền lưu lượng mTLS giữa ztunnel và Waypoint Proxy mà không cần triển khai sidecar trên từng Pod.

## Lợi ích khi kết hợp

Việc kết hợp Amazon EKS Auto Mode và Istio Ambient Mesh mang lại nhiều lợi ích:

- Giảm đáng kể công việc vận hành Kubernetes.
- Tự động bảo mật giao tiếp giữa các dịch vụ bằng mTLS.
- Giảm mức sử dụng CPU và bộ nhớ nhờ kiến trúc sidecarless.
- Tối ưu chi phí hạ tầng với cơ chế tự động mở rộng.
- Triển khai đơn giản bằng các công cụ như Terraform và Helm.

## Kiến trúc hệ thống

![Kiến trúc EKS Auto Mode và Istio Ambient Mesh](/images/3-BlogsPosted/3.1-Blog1/c-170-1.jpg)

## Tài liệu tham khảo

- AWS Containers Blog:
  https://aws.amazon.com/blogs/containers/better-together-amazon-eks-auto-mode-and-istio-ambient-mesh/