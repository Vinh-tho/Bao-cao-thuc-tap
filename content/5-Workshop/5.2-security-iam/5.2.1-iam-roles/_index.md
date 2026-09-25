---
title: "Assigning IAM Roles"
date: 2026-09-24
weight: 1
chapter: false
pre: " <b> 5.2.1. </b> "
---

# 5.2.1. Assigning IAM Roles for EC2, ECS Tasks, and Lambda

In AWS architecture, services do not automatically have permission to access one another. We need to create **IAM Roles** to authorize EC2 instances to communicate with ECS, ECS Tasks to pull Docker Images from ECR, and Lambda functions to process files on S3.

### Step 1: Create a Role for EC2 Instances (ECS Container Instance)

This role allows EC2 instances to automatically register to the ECS Cluster and send logs to the system.

1. In the search bar on the AWS Console, type **IAM** and select the **IAM** service.
2. In the left navigation pane, choose **Roles** and click **Create role**.
3. Under *Trusted entity type*, select **AWS service**. Under *Use case*, select **EC2** and click **Next**.

![Chọn Trusted Entity cho EC2](/images/5-Workshop/5.2/5.2.1/Screenshot%202026-09-25%20202833.png)
![Chọn Trusted Entity cho EC2](/images/5-Workshop/5.2/5.2.1/Screenshot%202026-09-25%20203141.png)
![Chọn Trusted Entity cho EC2](/images/5-Workshop/5.2/5.2.1/Screenshot%202026-09-25%20203259.png)

4. In the permissions policies search box, type `AmazonEC2ContainerServiceforEC2Role`. Check the box next to this policy and click **Next**.

![Chọn Policy cho EC2 ECS](/images/5-Workshop/5.2/5.2.1/Screenshot%202026-09-25%20203342.png)

5. On the *Name, review, and create* step, name the Role `Eshop-EC2-Instance-Role`. Click **Create role**.

![Tạo EC2 Role](/images/5-Workshop/5.2/5.2.1/Screenshot%202026-09-25%20203545.png)
![Tạo EC2 Role](/images/5-Workshop/5.2/5.2.1/Screenshot%202026-09-25%20203607.png)
![Tạo EC2 Role](/images/5-Workshop/5.2/5.2.1/Screenshot%202026-09-25%20203653.png)

### Step 2: Create a Role for ECS Task Execution

This role allows the Containers (Tasks) within ECS to pull images from Amazon ECR and push logs to CloudWatch.

1. Similarly, click **Create role** in the IAM console.
2. Select **AWS service**, scroll down to find and select **Elastic Container Service**. Under the detailed *Use case*, choose **Elastic Container Service Task**, then click **Next**.

![Chọn Trusted Entity cho ECS Task](/images/5-Workshop/5.2/5.2.1/Screenshot%202026-09-25%20204419.png)
![Chọn Trusted Entity cho ECS Task](/images/5-Workshop/5.2/5.2.1/Screenshot%202026-09-25%20204527.png)
![Chọn Trusted Entity cho ECS Task](/images/5-Workshop/5.2/5.2.1/Screenshot%202026-09-25%20204538.png)

3. Search for and check the policy `AmazonECSTaskExecutionRolePolicy`. Click **Next**.
4. Name the Role `Eshop-ECS-Task-Execution-Role`. Click **Create role**.

![Tạo ECS Task Role](/images/5-Workshop/5.2/5.2.1/Screenshot%202026-09-25%20204602.png)
![Tạo ECS Task Role](/images/5-Workshop/5.2/5.2.1/Screenshot%202026-09-25%20204623.png)
![Tạo ECS Task Role](/images/5-Workshop/5.2/5.2.1/Screenshot%202026-09-25%20204636.png)
![Tạo ECS Task Role](/images/5-Workshop/5.2/5.2.1/Screenshot%202026-09-25%20204645.png)


### Step 3: Create a Role for the AWS Lambda Function

This role grants the Lambda function permissions to read/write product images from S3 and write execution logs.

1. Click **Create role**.
2. Select **AWS service**, under *Use case* select **Lambda** and click **Next**.
3. Search for and check the following 2 policies:
   - `AWSLambdaBasicExecutionRole` (to write CloudWatch logs).
   - `AmazonS3FullAccess` (to retrieve and save resized images).
4. Click **Next**, name the Role `Eshop-Lambda-Image-Role`, and click **Create role**.

![Tạo Lambda Role](/images/5-Workshop/5.2/5.2.1/Screenshot%202026-09-25%20205059.png)
![Tạo Lambda Role](/images/5-Workshop/5.2/5.2.1/Screenshot%202026-09-25%20205202.png)
![Tạo Lambda Role](/images/5-Workshop/5.2/5.2.1/Screenshot%202026-09-25%20205207.png)
![Tạo Lambda Role](/images/5-Workshop/5.2/5.2.1/Screenshot%202026-09-25%20205253.png)
![Tạo Lambda Role](/images/5-Workshop/5.2/5.2.1/Screenshot%202026-09-25%20205308.png)
![Tạo Lambda Role](/images/5-Workshop/5.2/5.2.1/Screenshot%202026-09-25%20205328.png)
![Tạo Lambda Role](/images/5-Workshop/5.2/5.2.1/Screenshot%202026-09-25%20205338.png)
![Tạo Lambda Role](/images/5-Workshop/5.2/5.2.1/Screenshot%202026-09-25%20205348.png)