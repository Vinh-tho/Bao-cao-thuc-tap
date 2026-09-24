---
title: "Xây dựng Backend Container"
date: 2026-09-24
weight: 4
chapter: false
pre: " <b> 5.4. </b> "
---

# 5.4. Xây dựng Backend Container với Docker, Amazon EC2 & ECS

Sau khi Frontend đã hoạt động trên S3, chúng ta cần một Backend mạnh mẽ để xử lý các logic nghiệp vụ như: thêm vào giỏ hàng, thanh toán, tính toán khuyến mãi và truy xuất dữ liệu sản phẩm.

Thay vì chạy mã nguồn trực tiếp trên máy chủ ảo (EC2) truyền thống, chúng ta sẽ **Container hóa** Backend bằng Docker và triển khai lên **Amazon ECS (Elastic Container Service)**. Cách tiếp cận này giúp E-shop dễ dàng cập nhật phiên bản mới mà không lo lỗi môi trường (mất thư viện, sai phiên bản Node/Python) và đặc biệt là khả năng mở rộng (Auto Scaling) cực kỳ linh hoạt khi lượng truy cập tăng đột biến.

---

### Danh sách các bài thực hành chi tiết:

- **[5.4.1. Đóng gói Backend (Dockerfile) & Đẩy Image lên Amazon ECR](5.4.1-docker-ecr/)**
- **[5.4.2. Tạo EC2 Launch Template & EC2 Auto Scaling Group](5.4.2-ec2-asg/)**
- **[5.4.3. Khởi tạo ECS Cluster (EC2 Launch Type) & ECS Capacity Provider](5.4.3-ecs-cluster-cp/)**
- **[5.4.4. Cấu hình Application Load Balancer (ALB) & Target Group](5.4.4-alb-config/)**
- **[5.4.5. Định nghĩa ECS Task, Khởi tạo Service & Kết nối ALB](5.4.5-ecs-service/)**