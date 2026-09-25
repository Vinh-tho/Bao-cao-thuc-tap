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

![Chọn Trusted Entity cho EC2](/Bao-cao-thuc-tap/images/5-Workshop/5.2/5.2.1/Screenshot%202026-09-25%20202833.png)
![Chọn Trusted Entity cho EC2](/Bao-cao-thuc-tap/images/5-Workshop/5.2/5.2.1/Screenshot%202026-09-25%20203141.png)
![Chọn Trusted Entity cho EC2](/Bao-cao-thuc-tap/images/5-Workshop/5.2/5.2.1/Screenshot%202026-09-25%20203259.png)

4. Tại ô tìm kiếm chính sách (Permissions policies), gõ `AmazonEC2ContainerServiceforEC2Role`. Tích chọn chính sách này và nhấn **Next**.

![Chọn Policy cho EC2 ECS](/Bao-cao-thuc-tap/images/5-Workshop/5.2/5.2.1/Screenshot%202026-09-25%20203342.png)

5. Ở bước *Name, review, and create*, đặt tên Role là `Eshop-EC2-Instance-Role`. Nhấn **Create role**.

![Tạo EC2 Role](/Bao-cao-thuc-tap/images/5-Workshop/5.2/5.2.1/Screenshot%202026-09-25%20203545.png)
![Tạo EC2 Role](/Bao-cao-thuc-tap/images/5-Workshop/5.2/5.2.1/Screenshot%202026-09-25%20203607.png)
![Tạo EC2 Role](/Bao-cao-thuc-tap/images/5-Workshop/5.2/5.2.1/Screenshot%202026-09-25%20203653.png)

### Bước 2: Tạo Role cho ECS Task Execution

Role này cho phép các Container (Task) trong ECS có quyền kéo (pull) Image từ Amazon ECR và đẩy log lên CloudWatch.

1. Tương tự, nhấn **Create role** trong giao diện IAM.
2. Chọn **AWS service**, kéo xuống tìm và chọn **Elastic Container Service**. Ở phần *Use case* chi tiết, chọn **Elastic Container Service Task**, rồi nhấn **Next**.

![Chọn Trusted Entity cho ECS Task](/Bao-cao-thuc-tap/images/5-Workshop/5.2/5.2.1/Screenshot%202026-09-25%20204419.png)
![Chọn Trusted Entity cho ECS Task](/Bao-cao-thuc-tap/images/5-Workshop/5.2/5.2.1/Screenshot%202026-09-25%20204527.png)
![Chọn Trusted Entity cho ECS Task](/Bao-cao-thuc-tap/images/5-Workshop/5.2/5.2.1/Screenshot%202026-09-25%20204538.png)

3. Tìm và tích chọn chính sách `AmazonECSTaskExecutionRolePolicy`. Nhấn **Next**.
4. Đặt tên Role là `Eshop-ECS-Task-Execution-Role`. Nhấn **Create role**.

![Tạo ECS Task Role](/Bao-cao-thuc-tap/images/5-Workshop/5.2/5.2.1/Screenshot%202026-09-25%20204602.png)
![Tạo ECS Task Role](/Bao-cao-thuc-tap/images/5-Workshop/5.2/5.2.1/Screenshot%202026-09-25%20204623.png)
![Tạo ECS Task Role](/Bao-cao-thuc-tap/images/5-Workshop/5.2/5.2.1/Screenshot%202026-09-25%20204636.png)
![Tạo ECS Task Role](/Bao-cao-thuc-tap/images/5-Workshop/5.2/5.2.1/Screenshot%202026-09-25%20204645.png)

### Bước 3: Tạo Role cho hàm AWS Lambda

Role này cung cấp quyền cho hàm Lambda đọc/ghi ảnh sản phẩm từ S3 và ghi log chạy hàm.

1. Nhấn **Create role**.
2. Chọn **AWS service**, tại *Use case* chọn **Lambda** và nhấn **Next**.
3. Tìm và tích chọn 2 chính sách sau:
   - `AWSLambdaBasicExecutionRole` (để ghi log CloudWatch).
   - `AmazonS3FullAccess` (để lấy và lưu ảnh đã resize).
4. Nhấn **Next**, đặt tên Role là `Eshop-Lambda-Image-Role` và nhấn **Create role**.

![Tạo Lambda Role](/Bao-cao-thuc-tap/images/5-Workshop/5.2/5.2.1/Screenshot%202026-09-25%20205059.png)
![Tạo Lambda Role](/Bao-cao-thuc-tap/images/5-Workshop/5.2/5.2.1/Screenshot%202026-09-25%20205202.png)
![Tạo Lambda Role](/Bao-cao-thuc-tap/images/5-Workshop/5.2/5.2.1/Screenshot%202026-09-25%20205207.png)
![Tạo Lambda Role](/Bao-cao-thuc-tap/images/5-Workshop/5.2/5.2.1/Screenshot%202026-09-25%20205253.png)
![Tạo Lambda Role](/Bao-cao-thuc-tap/images/5-Workshop/5.2/5.2.1/Screenshot%202026-09-25%20205308.png)
![Tạo Lambda Role](/Bao-cao-thuc-tap/images/5-Workshop/5.2/5.2.1/Screenshot%202026-09-25%20205328.png)
![Tạo Lambda Role](/Bao-cao-thuc-tap/images/5-Workshop/5.2/5.2.1/Screenshot%202026-09-25%20205338.png)
![Tạo Lambda Role](/Bao-cao-thuc-tap/images/5-Workshop/5.2/5.2.1/Screenshot%202026-09-25%20205348.png)