---
title: "Dọn dẹp Tài nguyên"
date: 2026-09-24
weight: 7
chapter: false
pre: " <b> 3.7. </b> "
---

# 3.7. Dọn dẹp tài nguyên (Cleanup)

Quá trình triển khai dự án Web E-shop trên nền tảng AWS đã hoàn tất. Để ngăn ngừa việc phát sinh các khoản chi phí không mong muốn (đặc biệt đối với các dịch vụ tính phí theo thời gian chạy như NAT Gateway hoặc Application Load Balancer), quy trình dọn dẹp và thu hồi tài nguyên hệ thống là một yêu cầu bắt buộc.

Việc gỡ bỏ tài nguyên cần được thực hiện tuần tự theo đúng cấu trúc hướng dẫn dưới đây nhằm tránh các lỗi phát sinh do sự ràng buộc giữa các thành phần dịch vụ (Dependency error).

---

### Danh sách các nội dung dọn dẹp chi tiết:

- **[3.7.1. Dọn dẹp Application Load Balancer & Auto Scaling Group](3.7.1-alb-asg-cleanup/)**
- **[3.7.2. Dọn dẹp ECS Cluster, Task Definitions & Amazon ECR](3.7.2-ecs-ecr-cleanup/)**
- **[3.7.3. Dọn dẹp AWS Lambda & Amazon S3 Buckets](3.7.3-lambda-s3-cleanup/)**
- **[3.7.4. Dọn dẹp VPC, NAT Gateway & Elastic IP (Tránh phí phát sinh)](3.7.4-vpc-nat-cleanup/)**