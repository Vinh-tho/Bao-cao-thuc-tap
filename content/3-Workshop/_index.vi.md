---
title: "Workshop"
date: 2026-09-24
weight: 3
chapter: false
pre: " <b> 3. </b> "
---

# Hướng dẫn chi tiết Triển khai Web E-shop với kiến trúc ECS & Serverless trên AWS

#### Tổng quan bài Thực hành (Workshop)

Nội dung phần Workshop được cấu trúc thành các chương chính từ **3.1** đến **3.7** và các bài thực hành chi tiết **3.x.y** dưới đây, bám sát kiến trúc kết hợp giữa **Static Hosting (S3)**, **Container Backend (EC2/ECS)**, và **Event-Driven Serverless (Lambda)**:

> [!NOTE]

- **Link Web Demo**: [http://eshop-frontend-eshop-web.s3-website-ap-southeast-1.amazonaws.com](http://eshop-frontend-eshop-web.s3-website-ap-southeast-1.amazonaws.com)
- **Link Source Code**: [https://github.com/Vinh-tho/Eshop.git](https://github.com/Vinh-tho/Eshop.git)
- **Link Báo cáo thực tập**: [https://drive.google.com/file/d/1ruX8bMRgny0R4zI2cm051vuGI6jlASI8/view?usp=sharing](https://drive.google.com/file/d/1ruX8bMRgny0R4zI2cm051vuGI6jlASI8/view?usp=sharing)

---

#### Danh sách các chương thực hành:

1. [3.1. Xây dựng Nền tảng Mạng (Networking) với Amazon VPC](3.1-vpc-networking/)
   - [3.1.1. Khởi tạo VPC, Public Subnets và Private Subnets](3.1-vpc-networking/3.1.1-create-vpc-subnets/)
   - [3.1.2. Cấu hình Internet Gateway (IGW) và NAT Gateway](3.1-vpc-networking/3.1.2-igw-nat-gateway/)
   - [3.1.3. Cấu hình Route Tables cho luồng mạng](3.1-vpc-networking/3.1.3-route-tables/)
2. [3.2. Thiết lập Bảo mật Cơ bản (IAM & Security Groups)](3.2-security-iam/)
   - [3.2.1. Phân quyền IAM Roles cho EC2, ECS Task và Lambda](3.2-security-iam/3.2.1-iam-roles/)
   - [3.2.2. Khởi tạo Security Groups cho ALB và ECS Backend](3.2-security-iam/3.2.2-security-groups/)
3. [3.3. Triển khai Frontend & Lưu trữ Media với Amazon S3](3.3-s3-hosting-media/)
   - [3.3.1. Tạo S3 Bucket cho Frontend & Cấu hình Static Website Hosting](3.3-s3-hosting-media/3.3.1-s3-frontend-hosting/)
   - [3.3.2. Tạo S3 Bucket lưu trữ Media (Hình ảnh sản phẩm)](3.3-s3-hosting-media/3.3.2-s3-media-storage/)
   - [3.3.3. Tải mã nguồn Frontend và tài nguyên tĩnh lên S3](3.3-s3-hosting-media/3.3.3-deploy-frontend/)
4. [3.4. Xây dựng Backend Container với Docker, Amazon EC2 & ECS](3.4-ecs-backend/)
   - [3.4.1. Đóng gói Backend (Dockerfile) & Đẩy Image lên Amazon ECR](3.4-ecs-backend/3.4.1-docker-ecr/)
   - [3.4.2. Tạo EC2 Launch Template & EC2 Auto Scaling Group](3.4-ecs-backend/3.4.2-ec2-asg/)
   - [3.4.3. Khởi tạo ECS Cluster (EC2 Launch Type) & ECS Capacity Provider](3.4-ecs-backend/3.4.3-ecs-cluster-cp/)
   - [3.4.4. Cấu hình Application Load Balancer (ALB) & Target Group](3.4-ecs-backend/3.4.4-alb-config/)
   - [3.4.5. Định nghĩa ECS Task, Khởi tạo Service & Kết nối ALB](3.4-ecs-backend/3.4.5-ecs-service/)
5. [3.5. Xử lý Tác vụ Nền bằng Sự kiện (Event-Driven) với AWS Lambda](3.5-serverless-lambda/)
   - [3.3.1. Viết mã & Khởi tạo hàm AWS Lambda xử lý ảnh (Resize)](3.5-serverless-lambda/3.3.1-create-lambda/)
   - [3.3.2. Thiết lập S3 Event Notification kích hoạt Lambda](3.5-serverless-lambda/3.3.2-s3-event-trigger/)
   - [3.5.3. Kiểm thử luồng Upload ảnh và tự động Resize](3.5-serverless-lambda/3.5.3-test-workflow/)
6. [3.6. Giám sát & Tự động mở rộng (Monitoring & Auto Scaling)](3.6-monitoring-scaling/)
   - [3.6.1. Thiết lập CloudWatch Logs & Cảnh báo Alarms](3.6-monitoring-scaling/3.6.1-cloudwatch-alarms/)
   - [3.6.2. Cấu hình ECS Service Auto Scaling theo tải (Traffic/CPU)](3.6-monitoring-scaling/3.6.2-service-autoscaling/)
   - [3.6.3. Kích hoạt AWS CloudTrail để kiểm vết API](3.6-monitoring-scaling/3.6.3-cloudtrail-audit/)
7. [3.7. Dọn dẹp tài nguyên](3.7-cleanup/)
   - [3.7.1. Dọn dẹp Application Load Balancer & Auto Scaling Group](3.7-cleanup/3.7.1-alb-asg-cleanup/)
   - [3.7.2. Dọn dẹp ECS Cluster, Task Definitions & Amazon ECR](3.7-cleanup/3.7.2-ecs-ecr-cleanup/)
   - [3.7.3. Dọn dẹp AWS Lambda & Amazon S3 Buckets](3.7-cleanup/3.7.3-lambda-s3-cleanup/)
   - [3.7.4. Dọn dẹp VPC, NAT Gateway & Elastic IP (Tránh phí phát sinh)](3.7-cleanup/3.7.4-vpc-nat-cleanup/)
