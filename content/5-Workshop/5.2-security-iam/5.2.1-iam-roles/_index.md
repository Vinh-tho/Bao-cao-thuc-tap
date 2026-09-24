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

![Select Trusted Entity for EC2](/images/5-Workshop/5.2.1/iam_ec2_entity.png)

4. In the permissions policies search box, type `AmazonEC2ContainerServiceforEC2Role`. Check the box next to this policy and click **Next**.

![Select Policy for EC2 ECS](/images/5-Workshop/5.2.1/iam_ec2_policy.png)

5. On the *Name, review, and create* step, name the Role `Eshop-EC2-Instance-Role`. Click **Create role**.

![Create EC2 Role](/images/5-Workshop/5.2.1/iam_ec2_create.png)

### Step 2: Create a Role for ECS Task Execution

This role allows the Containers (Tasks) within ECS to pull images from Amazon ECR and push logs to CloudWatch.

1. Similarly, click **Create role** in the IAM console.
2. Select **AWS service**, scroll down to find and select **Elastic Container Service**. Under the detailed *Use case*, choose **Elastic Container Service Task**, then click **Next**.

![Select Trusted Entity for ECS Task](/images/5-Workshop/5.2.1/iam_ecs_entity.png)

3. Search for and check the policy `AmazonECSTaskExecutionRolePolicy`. Click **Next**.
4. Name the Role `Eshop-ECS-Task-Execution-Role`. Click **Create role**.

![Create ECS Task Role](/images/5-Workshop/5.2.1/iam_ecs_create.png)

### Step 3: Create a Role for the AWS Lambda Function

This role grants the Lambda function permissions to read/write product images from S3 and write execution logs.

1. Click **Create role**.
2. Select **AWS service**, under *Use case* select **Lambda** and click **Next**.
3. Search for and check the following 2 policies:
   - `AWSLambdaBasicExecutionRole` (to write CloudWatch logs).
   - `AmazonS3FullAccess` (to retrieve and save resized images).
4. Click **Next**, name the Role `Eshop-Lambda-Image-Role`, and click **Create role**.

![Create Lambda Role](/images/5-Workshop/5.2.1/iam_lambda_create.png)