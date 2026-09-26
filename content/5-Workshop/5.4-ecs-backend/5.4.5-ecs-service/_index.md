---
title: "Defining the ECS Task & Service"
date: 2026-09-24
weight: 5
chapter: false
pre: " <b> 5.4.5. </b> "
---

# 5.4.5. Defining the ECS Task, Creating the Service & Connecting the ALB

After completing the server infrastructure (ECS Cluster) and the load balancer, the next step is to define the container runtime configuration (Task Definition) and set up the service (ECS Service) to maintain the application's availability.

### Step 1: Create the Task Definition (Container Blueprint)

1. Access the **ECS** service, navigate to **Task definitions** in the left menu, and select **Create new task definition**.
2. Fill in the basic information:
   - **Task definition family**: `Eshop-Backend-Task`
3. In the **Infrastructure requirements** section:
   - **Launch type**: Select only **Amazon EC2 instances** (deselect AWS Fargate).
   - **Network mode**: Select **bridge**. (This setting is required to enable Dynamic Port Mapping on EC2, which allows ECS to automatically assign random ports to avoid conflicts).
   - **Task size**: Manually enter values suitable for the t2.micro server's limits: CPU = `0.5 vCPU`, Memory = `0.5 GB`.
   - **Task role & Task execution role**: Select `Eshop-ECS-Task-Execution-Role` for both fields (created in section 5.2.1).
4. In the **Container - 1** section:
   - **Name**: `eshop-backend-container`
   - **Image URI**: Click the **Browse ECR images** button. In the window that appears, select the `eshop-backend` repository and check the image tagged `latest`. Then, in the **Select image by** field at the bottom, choose the **Image tag** option and click **Select image**. The system will automatically fill in the complete image URI.
   - **Port mappings**:
     - **Container port**: `80` (or the port the Backend service is listening on).
     - **Host port**: Leave blank or enter `0` (to enable Dynamic Port Mapping).
     - **Protocol**: `TCP`.
5. Scroll to the bottom of the page and click **Create** to finish.

![Creating the ECS Task Definition](/images/5-Workshop/5.4/5.4.5/Screenshot%202026-09-26%20072944.png)
![Creating the ECS Task Definition](/images/5-Workshop/5.4/5.4.5/Screenshot%202026-09-26%20073008.png)
![Creating the ECS Task Definition](/images/5-Workshop/5.4/5.4.5/Screenshot%202026-09-26%20073505.png)
![Creating the ECS Task Definition](/images/5-Workshop/5.4/5.4.5/Screenshot%202026-09-26%20073524.png)
![Creating the ECS Task Definition](/images/5-Workshop/5.4/5.4.5/Screenshot%202026-09-26%20074548.png)
![Creating the ECS Task Definition](/images/5-Workshop/5.4/5.4.5/Screenshot%202026-09-26%20074625.png)
![Creating the ECS Task Definition](/images/5-Workshop/5.4/5.4.5/Screenshot%202026-09-26%20074726.png)
![Creating the ECS Task Definition](/images/5-Workshop/5.4/5.4.5/Screenshot%202026-09-26%20074736.png)
![Creating the ECS Task Definition](/images/5-Workshop/5.4/5.4.5/Screenshot%202026-09-26%20074756.png)

### Step 2: Create the ECS Service and Integrate the Load Balancer

1. On the **Clusters** interface, go into the `Eshop-ECS-Cluster` cluster.
2. Switch to the **Services** tab and select **Create**.
3. In the **Service details** section:
   - **Task definition family**: Select `Eshop-Backend-Task` (set up in Step 1).
   - **Service name**: Enter `Eshop-Backend-Service`.
4. In the **Environment** section:
   - Expand **Compute configuration - advanced**.
   - **Compute options**: Select **Capacity provider strategy**.
   - Select **Use custom (Advanced)**.
   - In the configuration table, change the Capacity provider from the default (`FARGATE`) to `Eshop-ECS-CP`.
5. In the **Deployment configuration** section:
   - **Desired tasks** (the desired number of Tasks/Containers to maintain): Enter `2`.
6. In the **Networking** section: no VPC/Subnet configuration is required since the Task is using `bridge` network mode.
7. In the **Load balancing - optional** section:
   - Check **Use load balancing**.
   - **VPC**: Select **`Eshop-VPC`**.
   - **Load balancer type**: Select **Application Load Balancer**.
   - In the **Container** field, select `eshop-backend-container 80:80`.
   - **Application Load Balancer**: Select **Use an existing load balancer**.
   - **Load balancer**: Open the list and select **`Eshop-ALB`**.
   - **Listener**: Select **Use an existing listener** and make sure port **`HTTP:80`** is selected.
   - **Target group**: Select **Use an existing target group** and specify **`Eshop-Backend-TG`**.
8. Scroll to the bottom of the page and click **Create** to finish the setup process.

![Creating the ECS Service](/images/5-Workshop/5.4/5.4.5/Screenshot%202026-09-26%20095119.png)
![Creating the ECS Service](/images/5-Workshop/5.4/5.4.5/Screenshot%202026-09-26%20095234.png)
![Creating the ECS Service](/images/5-Workshop/5.4/5.4.5/Screenshot%202026-09-26%20095255.png)
![Creating the ECS Service](/images/5-Workshop/5.4/5.4.5/Screenshot%202026-09-26%20095416.png)
![Creating the ECS Service](/images/5-Workshop/5.4/5.4.5/Screenshot%202026-09-26%20095837.png)

### Step 3: Check and Validate the System

1. Deploying the Service takes about 1-2 minutes. Progress can be monitored on the **Deployments** and **Tasks** tabs within the Cluster management interface.
2. Once the status of all Tasks changes to **Running**, the system is ready to accept requests.
3. Open a browser and navigate to the ALB's **DNS Name** (provided in section 5.4.5).
   - The setup is successfully verified when you receive a correct response from the Backend API (for example, JSON data returning a healthy status message). The Backend Container system has now been fully deployed and is capable of handling load.