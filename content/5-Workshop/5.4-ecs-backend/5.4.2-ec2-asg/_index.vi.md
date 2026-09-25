---
title: "Launch Template & ASG"
date: 2026-09-24
weight: 2
chapter: false
pre: " <b> 5.4.2. </b> "
---

# 5.4.2. Tạo EC2 Launch Template & EC2 Auto Scaling Group

Trong kiến trúc dự án, các Docker Container (ECS Task) được triển khai trên hạ tầng máy chủ EC2. Nhằm đảm bảo khả năng mở rộng tự động (Auto Scaling) khi lưu lượng truy cập thay đổi, hệ thống yêu cầu thiết lập **Launch Template** (Bản mẫu cấu hình) và **Auto Scaling Group - ASG** (Nhóm tự động mở rộng).

### Bước 1: Khởi tạo EC2 Launch Template

1. Truy cập dịch vụ **EC2** trên giao diện AWS Console. Điều hướng đến mục **Launch Templates** tại thanh menu bên trái và chọn **Create launch template**.
2. Khai báo thông tin cơ bản:
   - **Launch template name**: `Eshop-ECS-Launch-Template`
   - Bỏ qua cấu hình *Template version description*.
3. Tại phần **Application and OS Images (Amazon Machine Image)**: 
   - Tìm kiếm với từ khóa `ecs-optimized`.
   - Chọn tab **AWS Marketplace** hoặc **Community AMIs** để sử dụng bản `Amazon ECS-Optimized Amazon Linux 2 AMI` (AMI đã tích hợp sẵn Docker và ECS Agent).
4. **Instance type**: Chọn `t2.micro` (hoặc `t3.micro`) nhằm tối ưu hóa chi phí vận hành.
5. **Key pair (login)**: Chọn *Proceed without a key pair* (Do kiến trúc không yêu cầu truy cập SSH trực tiếp vào máy chủ).
6. **Network settings**: 
   - Bỏ qua thiết lập Subnet (Cấu hình này sẽ được chỉ định tại ASG).
   - **Security groups**: Chọn `Eshop-Backend-SG` (đã khởi tạo tại phần 5.2.2).
7. Tại phần **Advanced details**:
   - **IAM instance profile**: Chọn `Eshop-EC2-Instance-Role` (đã khởi tạo tại phần 5.2.1).
   - Cuộn xuống mục **User data**, bổ sung kịch bản (script) sau để cấu hình máy chủ EC2 tự động gia nhập vào ECS Cluster tương ứng:
     ```bash
     #!/bin/bash
     echo ECS_CLUSTER=Eshop-ECS-Cluster >> /etc/ecs/ecs.config
     ```
8. Chọn **Create launch template** để hoàn tất khởi tạo.

![Tạo EC2 Launch Template](/images/5-Workshop/5.4.2/create_launch_template.png)

### Bước 2: Thiết lập Auto Scaling Group (ASG)

1. Từ giao diện dịch vụ EC2, điều hướng đến **Auto Scaling Groups** tại menu bên trái và chọn **Create Auto Scaling group**.
2. **Bước 1 (Choose launch template)**: 
   - **Auto Scaling group name**: `Eshop-ECS-ASG`
   - **Launch template**: Chọn `Eshop-ECS-Launch-Template` vừa tạo. Nhấn **Next**.
3. **Bước 2 (Network)**:
   - **VPC**: Chọn `Eshop-VPC`.
   - **Availability Zones and subnets**: Chọn 2 Private Subnet (`Eshop-Private-Subnet-1` và `Eshop-Private-Subnet-2`). Nhấn **Next**.
4. **Bước 3 (Load balancing)**: Giữ nguyên tùy chọn *No load balancer* (Dịch vụ Load Balancer sẽ được tích hợp thông qua ECS ở bước sau). Nhấn **Next**.
5. **Bước 4 (Group size)**:
   - **Desired capacity** (Dung lượng mong muốn): `2`
   - **Minimum capacity** (Tối thiểu): `1`
   - **Maximum capacity** (Tối đa): `4`
6. Bỏ qua các cấu hình nâng cao khác, nhấn **Next** liên tục đến màn hình Review và chọn **Create Auto Scaling group**.

![Tạo Auto Scaling Group](/images/5-Workshop/5.4.2/create_asg.png)

Sau khi hoàn tất cấu hình, ASG sẽ tự động cung cấp 2 máy chủ EC2 bên trong vùng mạng Private Subnet. Hạ tầng này đã sẵn sàng để tiếp nhận và triển khai các container Backend.