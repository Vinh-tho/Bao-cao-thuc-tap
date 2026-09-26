---
title: "Launch Template & ASG"
date: 2026-09-24
weight: 2
chapter: false
pre: " <b> 5.4.2. </b> "
---

# 5.4.2. Creating an EC2 Launch Template & EC2 Auto Scaling Group

In the project architecture, Docker Containers (ECS Tasks) are deployed on EC2 server infrastructure. To ensure automatic scaling capability (Auto Scaling) when traffic load changes, the system requires setting up a **Launch Template** (configuration template) and an **Auto Scaling Group - ASG**.

### Step 1: Create an EC2 Launch Template

1. Access the **EC2** service on the AWS Console interface. Navigate to **Launch Templates** in the left menu bar and select **Create launch template**.
2. Declare the basic information:
   - **Launch template name**: `Eshop-ECS-Launch-Template`
   - Skip the *Template version description* field.
3. In the **Application and OS Images (Amazon Machine Image)** section:
   - Search using the keyword `ecs-optimized`.
   - Open the **Community AMIs** tab and select the **Amazon ECS-Optimized Amazon Linux AMI** provided by AWS (Verified provider). This is a specialized AMI that comes pre-integrated with Docker and the ECS Agent.
4. **Instance type**: Select `t2.micro` to optimize operating costs (included in the Free Tier).
5. **Key pair (login)**: Select *Don't include in launch template* (since the architecture does not require direct SSH access to the server).
6. **Network settings**:
   - Skip the Subnet configuration (this will be specified at the ASG level).
   - **Security groups**: Select `Eshop-Backend-SG` (created in section 5.2.2).
7. In the **Advanced details** section:
   - **IAM instance profile**: Select `Eshop-EC2-Instance-Role` (created in section 5.2.1).
   - Scroll down to the **User data** section and add the following script to configure the EC2 server to automatically join the corresponding ECS Cluster:
```bash
     #!/bin/bash
     echo ECS_CLUSTER=Eshop-ECS-Cluster >> /etc/ecs/ecs.config
```
8. Select **Create launch template** to complete the initialization.

![Creating EC2 Launch Template](/images/5-Workshop/5.4/5.4.2/Screenshot%202026-09-26%20055130.png)
![Creating EC2 Launch Template](/images/5-Workshop/5.4/5.4.2/Screenshot%202026-09-26%20055223.png)
![Creating EC2 Launch Template](/images/5-Workshop/5.4/5.4.2/Screenshot%202026-09-26%20060316.png)
![Creating EC2 Launch Template](/images/5-Workshop/5.4/5.4.2/Screenshot%202026-09-26%20060344.png)
![Creating EC2 Launch Template](/images/5-Workshop/5.4/5.4.2/Screenshot%202026-09-26%20060430.png)
![Creating EC2 Launch Template](/images/5-Workshop/5.4/5.4.2/Screenshot%202026-09-26%20060824.png)
![Creating EC2 Launch Template](/images/5-Workshop/5.4/5.4.2/Screenshot%202026-09-26%20061412.png)
![Creating EC2 Launch Template](/images/5-Workshop/5.4/5.4.2/Screenshot%202026-09-26%20061607.png)
![Creating EC2 Launch Template](/images/5-Workshop/5.4/5.4.2/Screenshot%202026-09-26%20061639.png)
![Creating EC2 Launch Template](/images/5-Workshop/5.4/5.4.2/Screenshot%202026-09-26%20061725.png)
![Creating EC2 Launch Template](/images/5-Workshop/5.4/5.4.2/Screenshot%202026-09-26%20061810.png)

### Step 2: Set Up the Auto Scaling Group (ASG)

1. From the EC2 service interface, navigate to **Auto Scaling Groups** in the left menu and select **Create Auto Scaling group**.
2. **Step 1 (Choose launch template)**:
   - **Auto Scaling group name**: `Eshop-ECS-ASG`
   - **Launch template**: Select the `Eshop-ECS-Launch-Template` you just created. Click **Next**.
3. **Step 2 (Choose instance launch options)**:
   - **Instance type requirements**: Select the **Manually add instance types** option and set the **Primary instance type** to `t2.micro`.
   - **Network > VPC**: Select `Eshop-VPC`.
   - **Availability Zones and subnets**: Select the 2 Private Subnets (`Eshop-Private-Subnet-1` and `Eshop-Private-Subnet-2`). Click **Next**.
4. **Step 3 (Load balancing)**: Keep the *No load balancer* option (the Load Balancer service will be integrated through ECS at a later step). Click **Next**.
5. **Step 4 (Configure group size and scaling)**:
   - **Desired capacity**: `2`
   - **Minimum capacity**: `1`
   - **Maximum capacity**: `4`
6. Skip the other advanced configurations, keep clicking **Next** until you reach the Review screen, then select **Create Auto Scaling group**.

![Creating Auto Scaling Group](/images/5-Workshop/5.4/5.4.2/Screenshot%202026-09-26%20062352.png)
![Creating Auto Scaling Group](/images/5-Workshop/5.4/5.4.2/Screenshot%202026-09-26%20062441.png)
![Creating Auto Scaling Group](/images/5-Workshop/5.4/5.4.2/Screenshot%202026-09-26%20063119.png)
![Creating Auto Scaling Group](/images/5-Workshop/5.4/5.4.2/Screenshot%202026-09-26%20063133.png)
![Creating Auto Scaling Group](/images/5-Workshop/5.4/5.4.2/Screenshot%202026-09-26%20063204.png)
![Creating Auto Scaling Group](/images/5-Workshop/5.4/5.4.2/Screenshot%202026-09-26%20063346.png)
![Creating Auto Scaling Group](/images/5-Workshop/5.4/5.4.2/Screenshot%202026-09-26%20063537.png)
![Creating Auto Scaling Group](/images/5-Workshop/5.4/5.4.2/Screenshot%202026-09-26%20064116.png)

Once the configuration is complete, the ASG will automatically provision 2 EC2 servers within the Private Subnet network. This infrastructure is now ready to receive and deploy the Backend containers.