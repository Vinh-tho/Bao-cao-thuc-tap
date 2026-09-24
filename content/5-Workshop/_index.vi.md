---
title: "Workshop"
date: 2026-09-24
weight: 5
chapter: false
pre: " <b> 5. </b> "
---

# Hướng dẫn chi tiết Triển khai Web E-shop với kiến trúc ECS & Serverless trên AWS

#### Tổng quan bài Thực hành (Workshop)

Nội dung phần Workshop được cấu trúc thành các chương chính từ **5.1** đến **5.7** và các bài thực hành chi tiết **5.x.y** dưới đây, bám sát kiến trúc kết hợp giữa **Static Hosting (S3)**, **Container Backend (EC2/ECS)**, và **Event-Driven Serverless (Lambda)**:

> [!NOTE]
> * **Link Web Demo**: [http://eshop-frontend-hosting-demo.s3-website-ap-southeast-1.amazonaws.com/](#) *(Link minh họa)*
> * **Link Source Code**: [https://github.com/YourOrganization/aws-eshop-workshop](#) *(Link minh họa)*

---

#### Danh sách các chương thực hành:

1. [5.1. Xây dựng Nền tảng Mạng (Networking) với Amazon VPC](5.1-vpc-networking/)
   * [5.1.1. Khởi tạo VPC, Public Subnets và Private Subnets](5.1-vpc-networking/5.1.1-create-vpc-subnets/)
   * [5.1.2. Cấu hình Internet Gateway (IGW) và NAT Gateway](5.1-vpc-networking/5.1.2-igw-nat-gateway/)
   * [5.1.3. Cấu hình Route Tables cho luồng mạng](5.1-vpc-networking/5.1.3-route-tables/)
2. [5.2. Thiết lập Bảo mật Cơ bản (IAM & Security Groups)](5.2-security-iam/)
   * [5.2.1. Phân quyền IAM Roles cho EC2, ECS Task và Lambda](5.2-security-iam/5.2.1-iam-roles/)
   * [5.2.2. Khởi tạo Security Groups cho ALB và ECS Backend](5.2-security-iam/5.2.2-security-groups/)
3. [5.3. Triển khai Frontend & Lưu trữ Media với Amazon S3](5.3-s3-hosting-media/)
   * [5.3.1. Tạo S3 Bucket cho Frontend & Cấu hình Static Website Hosting](5.3-s3-hosting-media/5.3.1-s3-frontend-hosting/)
   * [5.3.2. Tạo S3 Bucket lưu trữ Media (Hình ảnh sản phẩm)](5.3-s3-hosting-media/5.3.2-s3-media-storage/)
   * [5.3.3. Tải mã nguồn Frontend và tài nguyên tĩnh lên S3](5.3-s3-hosting-media/5.3.3-deploy-frontend/)
4. [5.4. Xây dựng Backend Container với Docker, Amazon EC2 & ECS](5.4-ecs-backend/)
   * [5.4.1. Đóng gói Backend (Dockerfile) & Đẩy Image lên Amazon ECR](5.4-ecs-backend/5.4.1-docker-ecr/)
   * [5.4.2. Tạo EC2 Launch Template & EC2 Auto Scaling Group](5.4-ecs-backend/5.4.2-ec2-asg/)
   * [5.4.3. Khởi tạo ECS Cluster (EC2 Launch Type) & ECS Capacity Provider](5.4-ecs-backend/5.4.3-ecs-cluster-cp/)
   * [5.4.4. Cấu hình Application Load Balancer (ALB) & Target Group](5.4-ecs-backend/5.4.4-alb-config/)
   * [5.4.5. Định nghĩa ECS Task, Khởi tạo Service & Kết nối ALB](5.4-ecs-backend/5.4.5-ecs-service/)
5. [5.5. Xử lý Tác vụ Nền bằng Sự kiện (Event-Driven) với AWS Lambda](5.5-serverless-lambda/)
   * [5.5.1. Viết mã & Khởi tạo hàm AWS Lambda xử lý ảnh (Resize)](5.5-serverless-lambda/5.5.1-create-lambda/)
   * [5.5.2. Thiết lập S3 Event Notification kích hoạt Lambda](5.5-serverless-lambda/5.5.2-s3-event-trigger/)
   * [5.5.3. Kiểm thử luồng Upload ảnh và tự động Resize](5.5-serverless-lambda/5.5.3-test-workflow/)
6. [5.6. Giám sát & Tự động mở rộng (Monitoring & Auto Scaling)](5.6-monitoring-scaling/)
   * [5.6.1. Thiết lập CloudWatch Logs & Cảnh báo Alarms](5.6-monitoring-scaling/5.6.1-cloudwatch-alarms/)
   * [5.6.2. Cấu hình ECS Service Auto Scaling theo tải (Traffic/CPU)](5.6-monitoring-scaling/5.6.2-service-autoscaling/)
   * [5.6.3. Kích hoạt AWS CloudTrail để kiểm vết API](5.6-monitoring-scaling/5.6.3-cloudtrail-audit/)
7. [5.7. Dọn dẹp tài nguyên](5.7-cleanup/)
   * [5.7.1. Dọn dẹp Application Load Balancer & Auto Scaling Group](5.7-cleanup/5.7.1-alb-asg-cleanup/)
   * [5.7.2. Dọn dẹp ECS Cluster, Task Definitions & Amazon ECR](5.7-cleanup/5.7.2-ecs-ecr-cleanup/)
   * [5.7.3. Dọn dẹp AWS Lambda & Amazon S3 Buckets](5.7-cleanup/5.7.3-lambda-s3-cleanup/)
   * [5.7.4. Dọn dẹp VPC, NAT Gateway & Elastic IP (Tránh phí phát sinh)](5.7-cleanup/5.7.4-vpc-nat-cleanup/)