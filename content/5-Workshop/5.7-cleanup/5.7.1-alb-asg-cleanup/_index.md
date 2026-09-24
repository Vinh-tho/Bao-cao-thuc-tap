---
title: "Cleanup ALB & ASG"
date: 2026-09-24
weight: 1
chapter: false
pre: " <b> 5.7.1. </b> "
---

# 5.7.1. Cleaning up Application Load Balancer & Auto Scaling Group

The first step in the cleanup process is to block Internet traffic from entering the system and terminate all running EC2 virtual servers to stop compute charges.

### Step 1: Delete Application Load Balancer (ALB) and Target Group

1. Navigate to the **EC2** service on the AWS Console.
2. In the left menu, scroll down to the **Load Balancing** section and select **Load Balancers**.
3. Select the `Eshop-ALB` Load Balancer, click **Actions** -> **Delete load balancer**. Confirm the deletion.
4. Next, select **Target Groups** from the left menu.
5. Select `Eshop-Backend-TG`, click **Actions** -> **Delete**. Confirm the deletion.

![Delete ALB and Target Group](/images/5-Workshop/5.7.1/delete_alb_tg.png)

### Step 2: Delete Auto Scaling Group (ASG) and Launch Template

1. Still in the EC2 console, scroll to the bottom of the left menu and select **Auto Scaling Groups**.
2. Select `Eshop-ECS-ASG` and click the **Delete** button. This process will take about 1-2 minutes because AWS must terminate the running EC2 instances inside it.
3. Move to the **Launch Templates** section in the left menu.
4. Select `Eshop-ECS-Launch-Template`, click **Actions** -> **Delete template**. Confirm the deletion.

![Delete ASG and Launch Template](/images/5-Workshop/5.7.1/delete_asg_lt.png)

Once the ASG is deleted, all EC2 instances will automatically vanish. Next, we will clean up the logical parts of the Backend, which are the ECS cluster and the ECR Image registry.