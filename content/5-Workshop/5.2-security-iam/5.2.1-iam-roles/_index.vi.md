---
title: "Phân quyền IAM Roles"
date: 2026-09-24
weight: 1
chapter: false
pre: " <b> 5.2.1. </b> "
---

# 5.2.1. Phân quyền IAM Roles cho EC2, ECS Task và Lambda

Trong kiến trúc AWS, các dịch vụ không tự động có quyền truy cập lẫn nhau. Chúng ta cần tạo các **IAM Roles** (Vai trò) để cấp quyền cho máy chủ EC2 giao tiếp với ECS, cho ECS Task tải Docker Image từ ECR, và cho hàm Lambda xử lý file trên S3.

### Bước 1: Tạo Role cho EC2 Instance (ECS Container Instance)

Role này giúp các máy chủ EC2 tự động đăng ký vào ECS Cluster và gửi log lên hệ thống.

1. Tại thanh tìm kiếm trên AWS Console, gõ **IAM** và chọn dịch vụ **IAM**.
2. Ở menu bên trái, chọn **Roles** và nhấn **Create role**.
3. Tại mục *Trusted entity type*, chọn **AWS service**. Tại mục *Use case*, chọn **EC2** và nhấn **Next**.

![Chọn Trusted Entity cho EC2](/images/5-Workshop/5.2.1/iam_ec2_entity.png)

4. Tại ô tìm kiếm chính sách (Permissions policies), gõ `AmazonEC2ContainerServiceforEC2Role`. Tích chọn chính sách này và nhấn **Next**.

![Chọn Policy cho EC2 ECS](/images/5-Workshop/5.2.1/iam_ec2_policy.png)

5. Ở bước *Name, review, and create*, đặt tên Role là `Eshop-EC2-Instance-Role`. Nhấn **Create role**.

![Tạo EC2 Role](/images/5-Workshop/5.2.1/iam_ec2_create.png)

### Bước 2: Tạo Role cho ECS Task Execution

Role này cho phép các Container (Task) trong ECS có quyền kéo (pull) Image từ Amazon ECR và đẩy log lên CloudWatch.

1. Tương tự, nhấn **Create role** trong giao diện IAM.
2. Chọn **AWS service**, kéo xuống tìm và chọn **Elastic Container Service**. Ở phần *Use case* chi tiết, chọn **Elastic Container Service Task**, rồi nhấn **Next**.

![Chọn Trusted Entity cho ECS Task](/images/5-Workshop/5.2.1/iam_ecs_entity.png)

3. Tìm và tích chọn chính sách `AmazonECSTaskExecutionRolePolicy`. Nhấn **Next**.
4. Đặt tên Role là `Eshop-ECS-Task-Execution-Role`. Nhấn **Create role**.

![Tạo ECS Task Role](/images/5-Workshop/5.2.1/iam_ecs_create.png)

### Bước 3: Tạo Role cho hàm AWS Lambda

Role này cung cấp quyền cho hàm Lambda đọc/ghi ảnh sản phẩm từ S3 và ghi log chạy hàm.

1. Nhấn **Create role**.
2. Chọn **AWS service**, tại *Use case* chọn **Lambda** và nhấn **Next**.
3. Tìm và tích chọn 2 chính sách sau:
   - `AWSLambdaBasicExecutionRole` (để ghi log CloudWatch).
   - `AmazonS3FullAccess` (để lấy và lưu ảnh đã resize).
4. Nhấn **Next**, đặt tên Role là `Eshop-Lambda-Image-Role` và nhấn **Create role**.

![Tạo Lambda Role](/images/5-Workshop/5.2.1/iam_lambda_create.png)