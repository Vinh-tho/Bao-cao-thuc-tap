---
title: "Thiết lập Bảo mật Cơ bản (IAM & SG)"
date: 2026-09-24
weight: 2
chapter: false
pre: " <b> 5.2. </b> "
---

# 5.2. Thiết lập Bảo mật Cơ bản (IAM & Security Groups)

Trong chương này, chúng ta sẽ cấu hình các lớp bảo mật nền tảng cho hệ thống Web E-shop theo nguyên tắc quyền tối thiểu (Least Privilege) của AWS. Cụ thể, chúng ta sẽ tạo các **IAM Roles** cho EC2, ECS Task và Lambda để chúng có quyền gọi các dịch vụ AWS khác (như S3, ECR, CloudWatch). Đồng thời, chúng ta thiết lập các **Security Groups** đóng vai trò như tường lửa ảo để kiểm soát chặt chẽ luồng truy cập mạng giữa Load Balancer ở Public Subnet và Backend Container ở Private Subnet.

---

### Danh sách các bài thực hành chi tiết:

- **[5.2.1. Phân quyền IAM Roles cho EC2, ECS Task và Lambda](5.2.1-iam-roles/)**
- **[5.2.2. Khởi tạo Security Groups cho ALB và ECS Backend](5.2.2-security-groups/)**