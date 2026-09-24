---
title: "Launch Template & ASG"
date: 2026-09-24
weight: 2
chapter: false
pre: " <b> 5.4.2. </b> "
---

# 5.4.2. Tạo EC2 Launch Template & EC2 Auto Scaling Group

Trong kiến trúc của chúng ta, các Docker Container (ECS Task) sẽ chạy trên nền các máy chủ EC2 ảo. Để hệ thống có thể tự động bật thêm máy chủ khi lượng khách hàng tăng đột biến (Flash Sale), chúng ta cần tạo **Launch Template** (Bản mẫu cấu hình máy chủ) và **Auto Scaling Group** (Nhóm tự động mở rộng).

### Bước 1: Tạo EC2 Launch Template

1. Truy cập dịch vụ **EC2** trên AWS Console. Ở menu bên trái, chọn **Launch Templates** và nhấn **Create launch template**.
2. Điền thông tin cơ bản:
   - **Launch template name**: `Eshop-ECS-Launch-Template`
   - Bỏ qua mục *Template version description*.
3. Tại mục **Application and OS Images (Amazon Machine Image)**: 
   - Nhấn vào ô tìm kiếm, gõ `ecs-optimized` và nhấn Enter.
   - Chọn tab **AWS Marketplace** hoặc **Community AMIs** để tìm `Amazon ECS-Optimized Amazon Linux 2 AMI` (Đây là hệ điều hành đã cài sẵn Docker và ECS Agent).
4. **Instance type**: Chọn `t2.micro` hoặc `t3.micro` (để tiết kiệm chi phí/Free Tier).
5. **Key pair (login)**: Chọn *Proceed without a key pair* (Vì chúng ta không cần SSH vào máy chủ).
6. **Network settings**: 
   - Không chọn Subnet ở đây (chúng ta sẽ cấu hình trong ASG).
   - **Security groups**: Chọn `Eshop-Backend-SG` (đã tạo ở bài 5.2.2).
7. Cuộn xuống mở phần **Advanced details**:
   - **IAM instance profile**: Chọn `Eshop-EC2-Instance-Role` (đã tạo ở bài 5.2.1).
   - Cuộn xuống dưới cùng tại mục **User data**, dán đoạn script sau để báo cho máy chủ EC2 biết nó cần gia nhập vào Cluster nào:
     ```bash
     #!/bin/bash
     echo ECS_CLUSTER=Eshop-ECS-Cluster >> /etc/ecs/ecs.config
     ```
8. Nhấn **Create launch template**.

![Tạo EC2 Launch Template](/images/5-Workshop/5.4.2/create_launch_template.png)

### Bước 2: Tạo Auto Scaling Group (ASG)

1. Vẫn ở giao diện EC2, nhìn menu bên trái dưới cùng chọn **Auto Scaling Groups** và nhấn **Create Auto Scaling group**.
2. **Bước 1 (Choose launch template)**: 
   - Name: `Eshop-ECS-ASG`
   - Launch template: Chọn `Eshop-ECS-Launch-Template`. Nhấn **Next**.
3. **Bước 2 (Network)**:
   - VPC: Chọn `Eshop-VPC`.
   - Availability Zones and subnets: Chọn **2 Private Subnets** (`Eshop-Private-Subnet-1` và `Eshop-Private-Subnet-2`). Nhấn **Next**.
4. **Bước 3 (Load balancing)**: Giữ nguyên *No load balancer* (ECS sẽ tự động gắn Load Balancer sau). Nhấn **Next**.
5. **Bước 4 (Group size)**:
   - Desired capacity (Số lượng mong muốn): `2`
   - Minimum capacity (Tối thiểu): `1`
   - Maximum capacity (Tối đa): `4`
   - Nhấn **Next** liên tục qua các bước còn lại và nhấn **Create Auto Scaling group**.

![Tạo Auto Scaling Group](/images/5-Workshop/5.4.2/create_asg.png)

Lúc này, ASG sẽ tự động khởi tạo 2 máy chủ EC2 nằm an toàn trong Private Subnet, sẵn sàng làm "nền móng" để chạy các container Backend ở bài sau.