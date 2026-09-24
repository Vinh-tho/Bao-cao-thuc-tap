---
title: "Dọn dẹp ALB & ASG"
date: 2026-09-24
weight: 1
chapter: false
pre: " <b> 5.7.1. </b> "
---

# 5.7.1. Dọn dẹp Application Load Balancer & Auto Scaling Group

Bước đầu tiên trong quá trình dọn dẹp là chặn luồng truy cập từ Internet vào hệ thống và tắt toàn bộ các máy chủ ảo EC2 đang chạy để ngừng tính phí compute.

### Bước 1: Xóa Application Load Balancer (ALB) và Target Group

1. Truy cập dịch vụ **EC2** trên AWS Console.
2. Ở menu bên trái, cuộn xuống phần **Load Balancing** và chọn **Load Balancers**.
3. Chọn Load Balancer `Eshop-ALB`, nhấn nút **Actions** -> **Delete load balancer**. Nhập xác nhận để xóa.
4. Tiếp tục chọn **Target Groups** ở menu bên trái.
5. Chọn `Eshop-Backend-TG`, nhấn **Actions** -> **Delete**. Xác nhận xóa.

![Xóa ALB và Target Group](/images/5-Workshop/5.7.1/delete_alb_tg.png)

### Bước 2: Xóa Auto Scaling Group (ASG) và Launch Template

1. Vẫn trong giao diện EC2, cuộn xuống cuối cùng ở menu trái và chọn **Auto Scaling Groups**.
2. Chọn `Eshop-ECS-ASG`, nhấn nút **Delete**. Quá trình này sẽ mất khoảng 1-2 phút vì AWS phải tiến hành tắt (terminate) các máy chủ EC2 đang chạy bên trong.
3. Chuyển sang phần **Launch Templates** ở menu bên trái.
4. Chọn `Eshop-ECS-Launch-Template`, nhấn **Actions** -> **Delete template**. Xác nhận xóa.

![Xóa ASG và Launch Template](/images/5-Workshop/5.7.1/delete_asg_lt.png)

Sau khi ASG bị xóa, toàn bộ các máy chủ EC2 sẽ tự động bốc hơi. Tiếp theo, chúng ta sẽ dọn dẹp phần logic của Backend là cụm ECS và kho chứa Image ECR.