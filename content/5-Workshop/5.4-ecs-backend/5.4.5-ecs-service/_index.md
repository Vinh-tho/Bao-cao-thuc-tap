---
title: "ECS Service and ALB"
date: 2026-09-24
weight: 5
chapter: false
pre: " <b> 5.4.5. </b> "
---

# 5.4.5. Defining the ECS Task, Creating a Service & Connecting the ALB

With the server infrastructure (ECS Cluster) and the gateway (Load Balancer) in place, the final step is to define how your Container should run (Task Definition) and instruct the system to keep it running continuously (Service).

### Step 1: Create a Task Definition (Container Blueprint)

1. Navigate to the **ECS** service, select **Task definitions** from the left menu, and click **Create new task definition**.
2. **Task definition family**: Name it `Eshop-Backend-Task`.
3. Under **Infrastructure requirements**:
   - **Launch type**: Select **Amazon EC2 instances**.
   - **Network mode**: Select **bridge** (This is crucial for ECS to automatically assign random ports (Dynamic Port Mapping) on the EC2 instances to avoid conflicts).
   - **Task size**: Memory = `512`, CPU = `0.5 vCPU`.
   - **Task role & Task execution role**: Select `Eshop-ECS-Task-Execution-Role` (Created in section 5.2.1).
4. Under **Container - 1**:
   - **Name**: `eshop-backend-container`
   - **Image URI**: Paste the URI of the Image you pushed to ECR in section 5.4.1.
   - **Port mappings**:
     - **Container port**: `80` (Or the port your Backend code is listening on).
     - **Host port**: Leave blank or enter `0` (To enable Dynamic Port Mapping).
     - **Protocol**: `TCP`.
5. Scroll to the bottom and click **Create**.

![Create ECS Task Definition](/images/5-Workshop/5.4.5/create_task_definition.png)

### Step 2: Create the ECS Service and Connect the Load Balancer

1. Return to the **Clusters** menu and click on `Eshop-ECS-Cluster`.
2. In the **Services** tab, click the **Create** button.
3. **Environment**:
   - Compute options: Select **Capacity provider strategy**.
   - Use custom strategy: Choose your Capacity Provider (e.g., `Eshop-ECS-ASG`).
4. **Deployment configuration**:
   - Application type: **Service**.
   - Family: Select `Eshop-Backend-Task` (created in Step 1).
   - Service name: `Eshop-Backend-Service`.
   - Desired tasks (Number of Containers to run): `2`.
5. **Networking**: Skip this section since we are using `bridge` mode.
6. **Load balancing**:
   - Load balancer type: Select **Application Load Balancer**.
   - Load balancer name: Select `Eshop-ALB`.
   - Under *Container to load balance*, select the `eshop-backend-container`.
   - Target group: Select **Use an existing target group** and choose `Eshop-Backend-TG` (created in section 5.4.4).
7. Scroll to the bottom and click **Create**.

![Create ECS Service](/images/5-Workshop/5.4.5/create_ecs_service.png)

### Step 3: Verify the Results

1. The Service deployment process may take 1-2 minutes. You can monitor the progress in the **Deployments** and **Tasks** tabs within your Cluster.
2. Once the tasks' statuses change to **Running**, retrieve the **DNS Name** of your ALB (which you saved in section 5.4.4).
3. Open a new browser tab, paste the ALB's DNS URL, and press Enter.
   - If you see a response from your Backend API, **CONGRATULATIONS!** Your Backend Container system is running perfectly!