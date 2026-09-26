---
title: "Configuring the ALB & Target Group"
date: 2026-09-26
weight: 4
chapter: false
pre: " <b> 5.4.4. </b> "
---

# 5.4.4. Configuring the Application Load Balancer (ALB) & Target Group

The Application Load Balancer (ALB) serves as the single entry point that receives incoming user traffic and evenly distributes the load across the Containers running on the ECS server cluster. For the ALB to route traffic correctly, a Target Group must be created first.

### Step 1: Create the Target Group

1. Access the **EC2** service, scroll down the left menu to find the **Load Balancing** section, and select **Target Groups**.
2. Click the **Create target group** button.
3. In the **Basic configuration** section:
   - **Choose a target type**: Select **Instances**. (Since the ECS `bridge` network mode is used, Amazon ECS will automatically register the EC2 servers into this group along with randomly assigned ports).
   - **Target group name**: Enter `Eshop-Backend-TG`.
   - **Protocol**: `HTTP`.
   - **Port**: `80`.
   - **VPC**: Select **`Eshop-VPC`** (the project's virtual network).
4. In the **Health checks** section:
   - **Health check protocol**: `HTTP`.
   - **Health check path**: `/` (or the Backend's health-check API path, if one exists).
5. Click **Next** to proceed to the next step.
6. On the *Register targets* screen, skip selecting any servers (the ECS Service will automatically handle this registration in section 5.4.5).
7. Scroll to the bottom of the page and click **Create target group**.

![Creating the Target Group](/images/5-Workshop/5.4/5.4.4/Screenshot%202026-09-26%20092628.png)
![Creating the Target Group](/images/5-Workshop/5.4/5.4.4/Screenshot%202026-09-26%20093045.png)
![Creating the Target Group](/images/5-Workshop/5.4/5.4.4/Screenshot%202026-09-26%20093110.png)
![Creating the Target Group](/images/5-Workshop/5.4/5.4.4/Screenshot%202026-09-26%20093205.png)

### Step 2: Create the Application Load Balancer (ALB)

1. In the left menu of the EC2 screen, select **Load Balancers**.
2. Click the **Create load balancer** button.
3. Under *Application Load Balancer*, click **Create**.
4. Fill in the **Basic configuration** section:
   - **Load balancer name**: Enter `Eshop-ALB`.
   - **Scheme**: Select **Internet-facing** (allows the ALB to accept traffic from the Internet).
   - **IP address type**: Select **IPv4**.
5. In the **Network mapping** section:
   - **VPC**: Select **`Eshop-VPC`**.
   - **Mappings**: Check at least **2 Availability Zones (AZs)** and select the corresponding **Public Subnets** to ensure High Availability.
6. In the **Security groups** section:
   - Remove the system's default `default` security group.
   - Assign the **`Eshop-ALB-SG`** security group (this group has already been configured to open port `80` for inbound external traffic).
7. In the **Listeners and routing** section:
   - **Protocol**: `HTTP`.
   - **Port**: `80`.
   - In the **Default action** (Forward to) field: open the dropdown list and select the **`Eshop-Backend-TG`** Target Group created in Step 1.
8. Scroll to the bottom of the page, review the *Summary* section, and click **Create load balancer**.

![Creating the Application Load Balancer](/images/5-Workshop/5.4/5.4.4/Screenshot%202026-09-26%20093523.png)
![Creating the Application Load Balancer](/images/5-Workshop/5.4/5.4.4/Screenshot%202026-09-26%20093620.png)
![Creating the Application Load Balancer](/images/5-Workshop/5.4/5.4.4/Screenshot%202026-09-26%20093738.png)
![Creating the Application Load Balancer](/images/5-Workshop/5.4/5.4.4/Screenshot%202026-09-26%20094031.png)
![Creating the Application Load Balancer](/images/5-Workshop/5.4/5.4.4/Screenshot%202026-09-26%20094239.png)
![Creating the Application Load Balancer](/images/5-Workshop/5.4/5.4.4/Screenshot%202026-09-26%20094407.png)

Creating the ALB will take about 2-3 minutes. The ALB's initial State will be *Provisioning*, and it is complete once it changes to **Active**. After this step, the deployment process will continue with Section 5.4.5 (Creating the ECS Service).