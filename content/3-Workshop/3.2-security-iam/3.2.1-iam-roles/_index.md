---
title: "Configuring IAM Roles"
date: 2026-09-24
weight: 1
chapter: false
pre: " <b> 3.2.1. </b> "
---

# 3.2.1. Configuring IAM Roles for EC2, ECS Task, and Lambda

In the AWS architecture, services are not granted access permissions to each other by default. Therefore, establishing **IAM Roles** is necessary to grant EC2 instances permission to communicate with ECS, allow ECS Tasks to pull Docker Images from ECR, and enable Lambda functions to process data on S3.

### Step 1: Creating a Role for EC2 Instances (ECS Container Instances)

This Role enables EC2 instances to automatically register with the ECS Cluster and push logs to the monitoring system.

1. In the search bar on the AWS Management Console, enter and select the **IAM** service.
2. In the left navigation pane, select **Roles** and click the **Create role** button.
3. Under the *Trusted entity type* section, select **AWS service**. Under the *Use case* section, select **EC2** and click **Next**.

![Selecting Trusted Entity for EC2](/images/3-Workshop/3.2/3.2.1/Screenshot%202026-09-25%20202833.png)
![Selecting Trusted Entity for EC2](/images/3-Workshop/3.2/3.2.1/Screenshot%202026-09-25%20203141.png)
![Selecting Trusted Entity for EC2](/images/3-Workshop/3.2/3.2.1/Screenshot%202026-09-25%20203259.png)

4. In the *Permissions policies* search box, enter `AmazonEC2ContainerServiceforEC2Role`. Check the box for this policy and click **Next**.

![Selecting Policy for EC2 ECS](/images/3-Workshop/3.2/3.2.1/Screenshot%202026-09-25%20203342.png)

5. In the *Name, review, and create* step, set the Role name to `Eshop-EC2-Instance-Role`. Click **Create role** to complete.

![Creating EC2 Role](/images/3-Workshop/3.2/3.2.1/Screenshot%202026-09-25%20203545.png)
![Creating EC2 Role](/images/3-Workshop/3.2/3.2.1/Screenshot%202026-09-25%20203607.png)
![Creating EC2 Role](/images/3-Workshop/3.2/3.2.1/Screenshot%202026-09-25%20203653.png)

### Step 2: Creating a Role for ECS Task Execution

This Role grants permissions for Containers (Tasks) within the ECS system to pull Images from Amazon ECR and push logs to CloudWatch.

1. Following a similar procedure to Step 1, click **Create role** in the IAM interface.
2. Select **AWS service**, search for and select **Elastic Container Service**. Under the specific *Use case* section, select **Elastic Container Service Task**, then click **Next**.

![Selecting Trusted Entity for ECS Task](/images/3-Workshop/3.2/3.2.1/Screenshot%202026-09-25%20204419.png)
![Selecting Trusted Entity for ECS Task](/images/3-Workshop/3.2/3.2.1/Screenshot%202026-09-25%20204527.png)
![Selecting Trusted Entity for ECS Task](/images/3-Workshop/3.2/3.2.1/Screenshot%202026-09-25%20204538.png)

3. Search for and check the `AmazonECSTaskExecutionRolePolicy` policy. Click **Next**.
4. Set the Role name to `Eshop-ECS-Task-Execution-Role` and click **Create role** to execute.

![Creating ECS Task Role](/images/3-Workshop/3.2/3.2.1/Screenshot%202026-09-25%20204602.png)
![Creating ECS Task Role](/images/3-Workshop/3.2/3.2.1/Screenshot%202026-09-25%20204623.png)
![Creating ECS Task Role](/images/3-Workshop/3.2/3.2.1/Screenshot%202026-09-25%20204636.png)
![Creating ECS Task Role](/images/3-Workshop/3.2/3.2.1/Screenshot%202026-09-25%20204645.png)

### Step 3: Creating a Role for AWS Lambda Functions

This Role provides the necessary access for the Lambda function to read/write product images from Amazon S3 and log execution metrics.

1. Click the **Create role** button.
2. Select **AWS service**, under *Use case* select **Lambda**, and click **Next**.
3. Search for and check the following 2 policies:
   - `AWSLambdaBasicExecutionRole` (Supports pushing logs to CloudWatch).
   - `AmazonS3FullAccess` (Grants access to retrieve and store data on S3).
4. Click **Next**, set the Role name to `Eshop-Lambda-Image-Role`, and click **Create role** to finalize.

![Creating Lambda Role](/images/3-Workshop/3.2/3.2.1/Screenshot%202026-09-25%20205059.png)
![Creating Lambda Role](/images/3-Workshop/3.2/3.2.1/Screenshot%202026-09-25%20205202.png)
![Creating Lambda Role](/images/3-Workshop/3.2/3.2.1/Screenshot%202026-09-25%20205207.png)
![Creating Lambda Role](/images/3-Workshop/3.2/3.2.1/Screenshot%202026-09-25%20205253.png)
![Creating Lambda Role](/images/3-Workshop/3.2/3.2.1/Screenshot%202026-09-25%20205308.png)
![Creating Lambda Role](/images/3-Workshop/3.2/3.2.1/Screenshot%202026-09-25%20205328.png)
![Creating Lambda Role](/images/3-Workshop/3.2/3.2.1/Screenshot%202026-09-25%20205338.png)
![Creating Lambda Role](/images/3-Workshop/3.2/3.2.1/Screenshot%202026-09-25%20205348.png)