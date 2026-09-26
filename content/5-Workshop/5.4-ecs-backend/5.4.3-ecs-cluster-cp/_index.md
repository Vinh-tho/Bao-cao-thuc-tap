---
title: "Creating the ECS Cluster"
date: 2026-09-24
weight: 3
chapter: false
pre: " <b> 5.4.3. </b> "
---

# 5.4.3. Creating an ECS Cluster (EC2 Launch Type) & ECS Capacity Provider

An **Amazon ECS Cluster** is the logical environment used to manage and orchestrate Docker Containers. By integrating this Cluster with the Auto Scaling Group (ASG) (set up in section 5.4.2) through a **Capacity Provider**, the ECS service is granted the ability to automatically scale, requesting EC2 to provision additional servers whenever the Containers need more resources (RAM/CPU) to handle a sudden spike in traffic.

### Step 1: Create the ECS Cluster

Since a newly created AWS account typically does not yet have the Service-Linked Roles for ECS initialized, creating the Cluster together with the Auto Scaling Group right from the start can cause an "Unable to assume the service linked role" error. Therefore, the process is split into two phases:

1. Access the **ECS (Elastic Container Service)** service on the AWS Console interface.
2. In the left navigation menu, select **Clusters** and click **Create cluster**.
3. In the **Cluster configuration** section: enter `Eshop-ECS-Cluster` as the **Cluster name**.
4. In the **Infrastructure** section: keep the default **Fargate only** option so the system automatically generates the required security Roles.
5. Click **Create** to complete the basic cluster creation.

![Creating ECS Cluster](/images/5-Workshop/5.4/5.4.3/Screenshot%202026-09-26%20064530.png)
![Creating ECS Cluster](/images/5-Workshop/5.4/5.4.3/Screenshot%202026-09-26%20064602.png)
![Creating ECS Cluster](/images/5-Workshop/5.4/5.4.3/Screenshot%202026-09-26%20070803.png)
![Creating ECS Cluster](/images/5-Workshop/5.4/5.4.3/Screenshot%202026-09-26%20070815.png)
![Creating ECS Cluster](/images/5-Workshop/5.4/5.4.3/Screenshot%202026-09-26%20070844.png)

### Step 2: Integrate the Auto Scaling Group (Capacity Provider)

1. Go to the detail page of the `Eshop-ECS-Cluster` you just created.
2. Switch to the **Infrastructure** tab.
3. In the **Capacity providers** section, click **Create**.
4. Enter the following information:
   - **Scaling type**: Select **EC2 Auto Scaling** to link with the manually created ASG.
   - **Capacity provider name**: Enter `Eshop-ECS-CP`.
   - **Auto Scaling group**: Select `Eshop-ECS-ASG` (the server group created in section 5.4.2).
5. Click **Create** and wait for the status to change to *Active*. This action grants the ECS cluster permission to use the EC2 servers managed by the ASG.

![Creating ECS Cluster linked with ASG](/images/5-Workshop/5.4/5.4.3/Screenshot%202026-09-26%20071101.png)
![Creating ECS Cluster linked with ASG](/images/5-Workshop/5.4/5.4.3/Screenshot%202026-09-26%20071547.png)
![Creating ECS Cluster linked with ASG](/images/5-Workshop/5.4/5.4.3/Screenshot%202026-09-26%20071619.png)


### Step 3: Verify the EC2 Instance Infrastructure

1. Still in the **Infrastructure** tab, scroll down to the **Container instances** section.
2. The system should show **2 EC2 servers** running with an *Active* status. This confirms that the EC2 servers have successfully and automatically joined the ECS cluster through the User Data configuration script.

![Checking ECS Cluster Infrastructure](/images/5-Workshop/5.4/5.4.3/Screenshot%202026-09-26%20071649.png)

The Backend server cluster infrastructure has now been fully prepared. In the next section, the system will be integrated with a Load Balancer, which will be responsible for routing incoming user traffic to this server cluster.