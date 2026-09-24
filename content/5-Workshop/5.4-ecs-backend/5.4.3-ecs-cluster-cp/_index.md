---
title: "Create ECS Cluster"
date: 2026-09-24
weight: 3
chapter: false
pre: " <b> 5.4.3. </b> "
---

# 5.4.3. Initializing an ECS Cluster (EC2 Launch Type) & ECS Capacity Provider

An **Amazon ECS Cluster** is a logical grouping used to manage Docker Containers. By connecting this Cluster to the Auto Scaling Group (ASG) created in section 5.4.2 via a **Capacity Provider**, ECS gains the authority to automatically request new EC2 instances when containers need more resources (RAM/CPU) to handle a massive influx of orders.

### Step 1: Initialize the ECS Cluster

1. Navigate to the **ECS (Elastic Container Service)** console on AWS.
2. In the left menu, select **Clusters** and click the **Create cluster** button.
3. Under **Cluster configuration**:
   - **Cluster name**: `Eshop-ECS-Cluster`
4. Under **Infrastructure**:
   - AWS Fargate (Serverless) is selected by default. However, our architecture uses EC2 to optimize costs according to project requirements.
   - Check the box for **Amazon EC2 instances**.
5. As soon as you select EC2, the **Auto Scaling group (ASG)** section will appear.
   - Select `Eshop-ECS-ASG` (the ASG we created in section 5.4.2) from the dropdown list.
   - Selecting the ASG directly here allows AWS to automatically create a **Capacity Provider** for you.
6. Scroll to the bottom and click **Create**.

![Initialize ECS Cluster connected to ASG](/images/5-Workshop/5.4.3/create_ecs_cluster.png)

### Step 2: Verify the Capacity Provider and EC2 Instances

Once the Cluster is successfully created (which takes about 1-2 minutes), we need to confirm that ECS has recognized the EC2 instances as its "workers."

1. Click on the `Eshop-ECS-Cluster` name to enter its details page.
2. Switch to the **Infrastructure** tab.
3. Scroll down to the **Capacity providers** section. You should see a newly auto-created provider (usually sharing the name of the ASG, with an *Active* status).
4. Scroll further down to the **Container instances** section. You should see **2 EC2 instances** with an *Active* status (These are the 2 servers provisioned by the ASG in section 5.4.2, which have successfully registered themselves into the ECS Cluster).

![Verify ECS Cluster infrastructure](/images/5-Workshop/5.4.3/verify_ecs_infrastructure.png)

Your Backend server cluster infrastructure is now ready! In the next section, we will set up the Load Balancer "Gateway" to direct customers into this cluster.