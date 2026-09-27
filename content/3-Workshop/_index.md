---
title: "Workshop"
date: 2026-09-24
weight: 3
chapter: false
pre: " <b> 3. </b> "
---

# Comprehensive Guide: Deploying a Web E-shop with ECS & Serverless Architecture on AWS

#### Workshop Overview

The Workshop content is structured into main chapters from **3.1** to **3.7**, along with detailed practical exercises **3.x.y** below. It adheres to a hybrid architecture that combines **Static Hosting (S3)**, **Container Backend (EC2/ECS)**, and **Event-Driven Serverless (Lambda)**:

> [!NOTE]

- **Link Web Demo**: [http://eshop-frontend-eshop-web.s3-website-ap-southeast-1.amazonaws.com](http://eshop-frontend-eshop-web.s3-website-ap-southeast-1.amazonaws.com)
- **Link Source Code**: [https://github.com/Vinh-tho/Eshop.git](https://github.com/Vinh-tho/Eshop.git)

---

#### List of Workshop Chapters:

1. [3.1. Building the Networking Foundation with Amazon VPC](3.1-vpc-networking/)
   - [3.1.1. Creating a VPC, Public Subnets, and Private Subnets](3.1-vpc-networking/3.1.1-create-vpc-subnets/)
   - [3.1.2. Configuring Internet Gateway (IGW) and NAT Gateway](3.1-vpc-networking/3.1.2-igw-nat-gateway/)
   - [3.1.3. Configuring Route Tables for Network Traffic](3.1-vpc-networking/3.1.3-route-tables/)
2. [3.2. Setting Up Basic Security (IAM & Security Groups)](3.2-security-iam/)
   - [3.2.1. Assigning IAM Roles for EC2, ECS Tasks, and Lambda](3.2-security-iam/3.2.1-iam-roles/)
   - [3.2.2. Creating Security Groups for ALB and ECS Backend](3.2-security-iam/3.2.2-security-groups/)
3. [3.3. Deploying Frontend & Media Storage with Amazon S3](3.3-s3-hosting-media/)
   - [3.3.1. Creating an S3 Bucket for Frontend & Configuring Static Website Hosting](3.3-s3-hosting-media/3.3.1-s3-frontend-hosting/)
   - [3.3.2. Creating an S3 Bucket for Media Storage (Product Images)](3.3-s3-hosting-media/3.3.2-s3-media-storage/)
   - [3.3.3. Uploading Frontend Source Code and Static Assets to S3](3.3-s3-hosting-media/3.3.3-deploy-frontend/)
4. [3.4. Building the Backend Container with Docker, Amazon EC2, & ECS](3.4-ecs-backend/)
   - [3.4.1. Packaging the Backend (Dockerfile) & Pushing the Image to Amazon ECR](3.4-ecs-backend/3.4.1-docker-ecr/)
   - [3.4.2. Creating an EC2 Launch Template & EC2 Auto Scaling Group](3.4-ecs-backend/3.4.2-ec2-asg/)
   - [3.4.3. Initializing an ECS Cluster (EC2 Launch Type) & ECS Capacity Provider](3.4-ecs-backend/3.4.3-ecs-cluster-cp/)
   - [3.4.4. Configuring the Application Load Balancer (ALB) & Target Group](3.4-ecs-backend/3.4.4-alb-config/)
   - [3.4.5. Defining the ECS Task, Creating a Service, & Connecting the ALB](3.4-ecs-backend/3.4.5-ecs-service/)
5. [3.5. Processing Background Tasks with Event-Driven AWS Lambda](3.5-serverless-lambda/)
   - [3.3.1. Writing Code & Creating an AWS Lambda Function for Image Resizing](3.5-serverless-lambda/3.3.1-create-lambda/)
   - [3.3.2. Setting Up S3 Event Notifications to Trigger Lambda](3.5-serverless-lambda/3.3.2-s3-event-trigger/)
   - [3.5.3. Testing the Image Upload and Auto-Resize Workflow](3.5-serverless-lambda/3.5.3-test-workflow/)
6. [3.6. Monitoring & Auto Scaling](3.6-monitoring-scaling/)
   - [3.6.1. Setting Up CloudWatch Logs & Alarms](3.6-monitoring-scaling/3.6.1-cloudwatch-alarms/)
   - [3.6.2. Configuring ECS Service Auto Scaling Based on Load (Traffic/CPU)](3.6-monitoring-scaling/3.6.2-service-autoscaling/)
   - [3.6.3. Enabling AWS CloudTrail for API Auditing](3.6-monitoring-scaling/3.6.3-cloudtrail-audit/)
7. [3.7. Resource Cleanup](3.7-cleanup/)
   - [3.7.1. Cleaning Up Application Load Balancer & Auto Scaling Group](3.7-cleanup/3.7.1-alb-asg-cleanup/)
   - [3.7.2. Cleaning Up ECS Cluster, Task Definitions, & Amazon ECR](3.7-cleanup/3.7.2-ecs-ecr-cleanup/)
   - [3.7.3. Cleaning Up AWS Lambda & Amazon S3 Buckets](3.7-cleanup/3.7.3-lambda-s3-cleanup/)
   - [3.7.4. Cleaning Up VPC, NAT Gateway, & Elastic IPs (To Avoid Extra Charges)](3.7-cleanup/3.7.4-vpc-nat-cleanup/)
