---
title: "Thiết lập Bảo mật Cơ bản (IAM & SG)"
date: 2026-09-24
weight: 2
chapter: false
pre: " <b> 3.2. </b> "
---

# 3.2. Thiết lập Bảo mật Cơ bản (IAM & Security Groups)

Chương này trình bày quy trình cấu hình các lớp bảo mật nền tảng cho hệ thống Web E-shop, tuân thủ nguyên tắc đặc quyền tối thiểu (Least Privilege) của AWS. Cụ thể, hệ thống sẽ khởi tạo các **IAM Roles** cho EC2, ECS Task và Lambda nhằm cấp quyền giao tiếp với các dịch vụ AWS khác (như S3, ECR, CloudWatch). Đồng thời, các **Security Groups** cũng được thiết lập để đóng vai trò như tường lửa ảo, giúp kiểm soát chặt chẽ luồng truy cập mạng giữa Load Balancer tại Public Subnet và Backend Container tại Private Subnet.

---

### Danh sách các bài thực hành chi tiết:

- **[3.2.1. Phân quyền IAM Roles cho EC2, ECS Task và Lambda](3.2.1-iam-roles/)**
- **[3.2.2. Khởi tạo Security Groups cho ALB và ECS Backend](3.2.2-security-groups/)**