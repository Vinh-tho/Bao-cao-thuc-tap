---
title: "Dọn dẹp Tài nguyên"
date: 2026-09-24
weight: 7
chapter: false
pre: " <b> 5.7. </b> "
---

# 5.7. Dọn dẹp tài nguyên (Cleanup)

Chúc mừng bạn đã hoàn thành trọn vẹn dự án Web E-shop trên AWS! Để tránh phát sinh chi phí không mong muốn (đặc biệt là các dịch vụ tính phí theo giờ như NAT Gateway hay Load Balancer), việc dọn dẹp hệ thống là bước bắt buộc.

Hãy thực hiện xóa tài nguyên lần lượt theo đúng thứ tự các bài dưới đây để tránh gặp lỗi ràng buộc (Dependency error).

---

### Danh sách các bài thực hành dọn dẹp:

- **[5.7.1. Dọn dẹp Application Load Balancer & Auto Scaling Group](5.7.1-alb-asg-cleanup/)**
- **[5.7.2. Dọn dẹp ECS Cluster, Task Definitions & Amazon ECR](5.7.2-ecs-ecr-cleanup/)**
- **[5.7.3. Dọn dẹp AWS Lambda & Amazon S3 Buckets](5.7.3-lambda-s3-cleanup/)**
- **[5.7.4. Dọn dẹp VPC, NAT Gateway & Elastic IP (Tránh phí phát sinh)](5.7.4-vpc-nat-cleanup/)**