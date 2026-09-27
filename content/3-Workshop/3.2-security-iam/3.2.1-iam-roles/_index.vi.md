---
title: "Phân quyền IAM Roles"
date: 2026-09-24
weight: 1
chapter: false
pre: " <b> 3.2.1. </b> "
---

# 3.2.1. Phân quyền IAM Roles cho EC2, ECS Task và Lambda

Trong kiến trúc AWS, các dịch vụ không được mặc định cấp quyền truy cập lẫn nhau. Do đó, việc thiết lập các **IAM Roles** (Vai trò) là cần thiết nhằm cấp quyền cho máy chủ EC2 giao tiếp với ECS, cho phép ECS Task tải (pull) Docker Image từ ECR, và cho phép hàm Lambda xử lý dữ liệu trên S3.

### Bước 1: Khởi tạo Role cho EC2 Instance (ECS Container Instance)

Role này cho phép các máy chủ EC2 tự động đăng ký vào ECS Cluster và đẩy log lên hệ thống giám sát.

1. Tại thanh tìm kiếm trên giao diện AWS Management Console, nhập và chọn dịch vụ **IAM**.
2. Tại thanh điều hướng bên trái, chọn **Roles** và nhấn nút **Create role**.
3. Tại phần *Trusted entity type*, chọn **AWS service**. Tại phần *Use case*, chọn **EC2** và nhấn **Next**.

![Chọn Trusted Entity cho EC2](/images/3-Workshop/3.2/3.2.1/Screenshot%202026-09-25%20202833.png)
![Chọn Trusted Entity cho EC2](/images/3-Workshop/3.2/3.2.1/Screenshot%202026-09-25%20203141.png)
![Chọn Trusted Entity cho EC2](/images/3-Workshop/3.2/3.2.1/Screenshot%202026-09-25%20203259.png)

4. Tại ô tìm kiếm *Permissions policies*, nhập `AmazonEC2ContainerServiceforEC2Role`. Đánh dấu chọn chính sách này và nhấn **Next**.

![Chọn Policy cho EC2 ECS](/images/3-Workshop/3.2/3.2.1/Screenshot%202026-09-25%20203342.png)

5. Tại bước *Name, review, and create*, đặt tên Role là `Eshop-EC2-Instance-Role`. Nhấn **Create role** để hoàn tất.

![Tạo EC2 Role](/images/3-Workshop/3.2/3.2.1/Screenshot%202026-09-25%20203545.png)
![Tạo EC2 Role](/images/3-Workshop/3.2/3.2.1/Screenshot%202026-09-25%20203607.png)
![Tạo EC2 Role](/images/3-Workshop/3.2/3.2.1/Screenshot%202026-09-25%20203653.png)

### Bước 2: Khởi tạo Role cho ECS Task Execution

Role này cấp quyền cho các Container (Task) trong hệ thống ECS tải (pull) Image từ Amazon ECR và ghi log lên CloudWatch.

1. Thực hiện tương tự Bước 1, nhấn **Create role** tại giao diện IAM.
2. Chọn **AWS service**, tìm và chọn **Elastic Container Service**. Tại mục *Use case* chi tiết, chọn **Elastic Container Service Task**, sau đó nhấn **Next**.

![Chọn Trusted Entity cho ECS Task](/images/3-Workshop/3.2/3.2.1/Screenshot%202026-09-25%20204419.png)
![Chọn Trusted Entity cho ECS Task](/images/3-Workshop/3.2/3.2.1/Screenshot%202026-09-25%20204527.png)
![Chọn Trusted Entity cho ECS Task](/images/3-Workshop/3.2/3.2.1/Screenshot%202026-09-25%20204538.png)

3. Tìm và đánh dấu chọn chính sách `AmazonECSTaskExecutionRolePolicy`. Nhấn **Next**.
4. Đặt tên Role là `Eshop-ECS-Task-Execution-Role` và nhấn **Create role** để thực thi.

![Tạo ECS Task Role](/images/3-Workshop/3.2/3.2.1/Screenshot%202026-09-25%20204602.png)
![Tạo ECS Task Role](/images/3-Workshop/3.2/3.2.1/Screenshot%202026-09-25%20204623.png)
![Tạo ECS Task Role](/images/3-Workshop/3.2/3.2.1/Screenshot%202026-09-25%20204636.png)
![Tạo ECS Task Role](/images/3-Workshop/3.2/3.2.1/Screenshot%202026-09-25%20204645.png)

### Bước 3: Khởi tạo Role cho hàm AWS Lambda

Role này cung cấp quyền truy cập để hàm Lambda thực hiện đọc/ghi hình ảnh sản phẩm từ Amazon S3, đồng thời ghi log quá trình thực thi.

1. Nhấn nút **Create role**.
2. Chọn **AWS service**, tại mục *Use case* chọn **Lambda** và nhấn **Next**.
3. Tìm và đánh dấu chọn 2 chính sách sau:
   - `AWSLambdaBasicExecutionRole` (Hỗ trợ ghi log lên CloudWatch).
   - `AmazonS3FullAccess` (Cấp quyền truy xuất và lưu trữ dữ liệu trên S3).
4. Nhấn **Next**, đặt tên Role là `Eshop-Lambda-Image-Role` và nhấn **Create role** để hoàn tất.

![Tạo Lambda Role](/images/3-Workshop/3.2/3.2.1/Screenshot%202026-09-25%20205059.png)
![Tạo Lambda Role](/images/3-Workshop/3.2/3.2.1/Screenshot%202026-09-25%20205202.png)
![Tạo Lambda Role](/images/3-Workshop/3.2/3.2.1/Screenshot%202026-09-25%20205207.png)
![Tạo Lambda Role](/images/3-Workshop/3.2/3.2.1/Screenshot%202026-09-25%20205253.png)
![Tạo Lambda Role](/images/3-Workshop/3.2/3.2.1/Screenshot%202026-09-25%20205308.png)
![Tạo Lambda Role](/images/3-Workshop/3.2/3.2.1/Screenshot%202026-09-25%20205328.png)
![Tạo Lambda Role](/images/3-Workshop/3.2/3.2.1/Screenshot%202026-09-25%20205338.png)
![Tạo Lambda Role](/images/3-Workshop/3.2/3.2.1/Screenshot%202026-09-25%20205348.png)