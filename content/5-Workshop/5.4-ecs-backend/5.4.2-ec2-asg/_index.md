---
title: "Launch Template & ASG"
date: 2026-09-24
weight: 2
chapter: false
pre: " <b> 5.4.2. </b> "
---

# 5.4.2. Creating an EC2 Launch Template & EC2 Auto Scaling Group

In our architecture, the Docker Containers (ECS Tasks) will run on underlying EC2 virtual servers. For the system to automatically spin up additional servers during traffic spikes (like a Flash Sale), we need to create a **Launch Template** (server configuration blueprint) and an **Auto Scaling Group** (ASG).

### Step 1: Create the EC2 Launch Template

1. Navigate to the **EC2** service on the AWS Console. In the left menu, choose **Launch Templates** and click **Create launch template**.
2. Fill in the basic details:
   - **Launch template name**: `Eshop-ECS-Launch-Template`
   - You can leave *Template version description* blank.
3. Under **Application and OS Images (Amazon Machine Image)**:
   - Click the search bar, type `ecs-optimized` and press Enter.
   - Select the **AWS Marketplace** or **Community AMIs** tab to find the `Amazon ECS-Optimized Amazon Linux 2 AMI` (This OS comes pre-installed with Docker and the ECS Agent).
4. **Instance type**: Select `t2.micro` or `t3.micro` (for cost savings/Free Tier).
5. **Key pair (login)**: Select *Proceed without a key pair* (Since we won't need to SSH into these servers).
6. **Network settings**:
   - Do not select a Subnet here (we will configure this in the ASG).
   - **Security groups**: Select `Eshop-Backend-SG` (created in section 5.2.2).
7. Scroll down and expand **Advanced details**:
   - **IAM instance profile**: Select `Eshop-EC2-Instance-Role` (created in section 5.2.1).
   - Scroll to the very bottom to the **User data** section and paste the following script to tell the EC2 instance which Cluster to join:
     ```bash
     #!/bin/bash
     echo ECS_CLUSTER=Eshop-ECS-Cluster >> /etc/ecs/ecs.config
     ```
8. Click **Create launch template**.

![Create EC2 Launch Template](/images/5-Workshop/5.4.2/create_launch_template.png)

### Step 2: Create the Auto Scaling Group (ASG)

1. Still in the EC2 console, look at the bottom left menu, select **Auto Scaling Groups**, and click **Create Auto Scaling group**.
2. **Step 1 (Choose launch template)**:
   - Name: `Eshop-ECS-ASG`
   - Launch template: Choose `Eshop-ECS-Launch-Template`. Click **Next**.
3. **Step 2 (Network)**:
   - VPC: Select `Eshop-VPC`.
   - Availability Zones and subnets: Select the **2 Private Subnets** (`Eshop-Private-Subnet-1` and `Eshop-Private-Subnet-2`). Click **Next**.
4. **Step 3 (Load balancing)**: Leave it as *No load balancer* (ECS will handle target group attachment later). Click **Next**.
5. **Step 4 (Group size)**:
   - Desired capacity: `2`
   - Minimum capacity: `1`
   - Maximum capacity: `4`
   - Keep clicking **Next** through the remaining steps, then click **Create Auto Scaling group**.

![Create Auto Scaling Group](/images/5-Workshop/5.4.2/create_asg.png)

At this point, the ASG will automatically provision 2 EC2 instances safely inside your Private Subnets. These will serve as the foundation to run your Backend containers in the upcoming steps.