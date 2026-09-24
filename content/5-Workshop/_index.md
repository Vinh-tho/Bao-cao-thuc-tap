---
title: "Workshop"
date: 2026-09-24
weight: 5
chapter: false
pre: " <b> 5. </b> "
---

# Comprehensive Guide: Deploying a Web E-shop with ECS & Serverless Architecture on AWS

#### Workshop Overview

The Workshop content is structured into main chapters from **5.1** to **5.7**, along with detailed practical exercises **5.x.y** below. It adheres to a hybrid architecture that combines **Static Hosting (S3)**, **Container Backend (EC2/ECS)**, and **Event-Driven Serverless (Lambda)**:

> [!NOTE]
> * **Web Demo Link**: [http://eshop-frontend-hosting-demo.s3-website-ap-southeast-1.amazonaws.com/](#) *(Example Link)*
> * **Source Code Link**: [https://github.com/YourOrganization/aws-eshop-workshop](#) *(Example Link)*

---

#### List of Workshop Chapters:

1. [5.1. Building the Networking Foundation with Amazon VPC](5.1-vpc-networking/)
   * [5.1.1. Creating a VPC, Public Subnets, and Private Subnets](5.1-vpc-networking/5.1.1-create-vpc-subnets/)
   * [5.1.2. Configuring Internet Gateway (IGW) and NAT Gateway](5.1-vpc-networking/5.1.2-igw-nat-gateway/)
   * [5.1.3. Configuring Route Tables for Network Traffic](5.1-vpc-networking/5.1.3-route-tables/)
2. [5.2. Setting Up Basic Security (IAM & Security Groups)](5.2-security-iam/)
   * [5.2.1. Assigning IAM Roles for EC2, ECS Tasks, and Lambda](5.2-security-iam/5.2.1-iam-roles/)
   * [5.2.2. Creating Security Groups for ALB and ECS Backend](5.2-security-iam/5.2.2-security-groups/)
3. [5.3. Deploying Frontend & Media Storage with Amazon S3](5.3-s3-hosting-media/)
   * [5.3.1. Creating an S3 Bucket for Frontend & Configuring Static Website Hosting](5.3-s3-hosting-media/5.3.1-s3-frontend-hosting/)
   * [5.3.2. Creating an S3 Bucket for Media Storage (Product Images)](5.3-s3-hosting-media/5.3.2-s3-media-storage/)
   * [5.3.3. Uploading Frontend Source Code and Static Assets to S3](5.3-s3-hosting-media/5.3.3-deploy-frontend/)
4. [5.4. Building the Backend Container with Docker, Amazon EC2, & ECS](5.4-ecs-backend/)
   * [5.4.1. Packaging the Backend (Dockerfile) & Pushing the Image to Amazon ECR](5.4-ecs-backend/5.4.1-docker-ecr/)
   * [5.4.2. Creating an EC2 Launch Template & EC2 Auto Scaling Group](5.4-ecs-backend/5.4.2-ec2-asg/)
   * [5.4.3. Initializing an ECS Cluster (EC2 Launch Type) & ECS Capacity Provider](5.4-ecs-backend/5.4.3-ecs-cluster-cp/)
   * [5.4.4. Configuring the Application Load Balancer (ALB) & Target Group](5.4-ecs-backend/5.4.4-alb-config/)
   * [5.4.5. Defining the ECS Task, Creating a Service, & Connecting the ALB](5.4-ecs-backend/5.4.5-ecs-service/)
5. [5.5. Processing Background Tasks with Event-Driven AWS Lambda](5.5-serverless-lambda/)
   * [5.5.1. Writing Code & Creating an AWS Lambda Function for Image Resizing](5.5-serverless-lambda/5.5.1-create-lambda/)
   * [5.5.2. Setting Up S3 Event Notifications to Trigger Lambda](5.5-serverless-lambda/5.5.2-s3-event-trigger/)
   * [5.5.3. Testing the Image Upload and Auto-Resize Workflow](5.5-serverless-lambda/5.5.3-test-workflow/)
6. [5.6. Monitoring & Auto Scaling](5.6-monitoring-scaling/)
   * [5.6.1. Setting Up CloudWatch Logs & Alarms](5.6-monitoring-scaling/5.6.1-cloudwatch-alarms/)
   * [5.6.2. Configuring ECS Service Auto Scaling Based on Load (Traffic/CPU)](5.6-monitoring-scaling/5.6.2-service-autoscaling/)
   * [5.6.3. Enabling AWS CloudTrail for API Auditing](5.6-monitoring-scaling/5.6.3-cloudtrail-audit/)
7. [5.7. Resource Cleanup](5.7-cleanup/)
   * [5.7.1. Cleaning Up Application Load Balancer & Auto Scaling Group](5.7-cleanup/5.7.1-alb-asg-cleanup/)
   * [5.7.2. Cleaning Up ECS Cluster, Task Definitions, & Amazon ECR](5.7-cleanup/5.7.2-ecs-ecr-cleanup/)
   * [5.7.3. Cleaning Up AWS Lambda & Amazon S3 Buckets](5.7-cleanup/5.7.3-lambda-s3-cleanup/)
   * [5.7.4. Cleaning Up VPC, NAT Gateway, & Elastic IPs (To Avoid Extra Charges)](5.7-cleanup/5.7.4-vpc-nat-cleanup/)