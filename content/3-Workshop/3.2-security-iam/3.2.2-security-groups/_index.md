---
title: "Creating Security Groups"
date: 2026-09-24
weight: 2
chapter: false
pre: " <b> 3.2.2. </b> "
---

# 3.2.2. Creating Security Groups for ALB and ECS Backend

A **Security Group (SG)** acts as a virtual firewall at the Instance level to control inbound and outbound network traffic. To adhere to strict security principles, the system architecture requires the creation of 2 SGs:
1. **ALB Security Group**: Allows Internet traffic to access the web application.
2. **Backend Security Group**: Completely isolated from the Internet, accepting only traffic forwarded from the ALB SG.

### Step 1: Creating a Security Group for the Application Load Balancer (ALB)

1. On the AWS Management Console, access the **VPC** (or EC2) service. In the left navigation pane, scroll down to the *Security* section and select **Security Groups**.
2. Click the **Create security group** button.
3. Configure the basic information:
   - **Security group name**: `Eshop-ALB-SG`
   - **Description**: `Allow HTTP and HTTPS traffic from Internet to ALB`
   - **VPC**: Click the `X` to remove the default VPC, then select `Eshop-VPC`.

![Configuring ALB Security Group Information](/images/3-Workshop/3.2/3.2.2/Screenshot%202026-09-25%20210249.png)
![Configuring ALB Security Group Information](/images/3-Workshop/3.2/3.2.2/Screenshot%202026-09-25%20210335.png)

4. Under the **Inbound rules** section, click **Add rule** twice to configure the web connection ports:
   - Rule 1: Type = `HTTP`, Source = `Anywhere-IPv4` (`0.0.0.0/0`)
   - Rule 2: Type = `HTTPS`, Source = `Anywhere-IPv4` (`0.0.0.0/0`)
5. Leave the **Outbound rules** as default (allowing All traffic) and click the **Create security group** button at the bottom of the page to execute.

![Adding Inbound Rules for ALB](/images/3-Workshop/3.2/3.2.2/Screenshot%202026-09-25%20210441.png)
![Adding Inbound Rules for ALB](/images/3-Workshop/3.2/3.2.2/Screenshot%202026-09-25%20210507.png)
![Adding Inbound Rules for ALB](/images/3-Workshop/3.2/3.2.2/Screenshot%202026-09-25%20210519.png)

### Step 2: Creating a Security Group for the Backend (EC2 / ECS Task)

The next step involves creating an SG for the Backend. The critical security requirement here is that the Backend must NOT open connections to the Internet (`0.0.0.0/0`). Instead, it will only accept traffic originating from the ALB's SG (Source).

1. Return to the Security Groups list and click **Create security group**.
2. Configure the basic information:
   - **Security group name**: `Eshop-Backend-SG`
   - **Description**: `Allow traffic only from ALB`
   - **VPC**: Select `Eshop-VPC`.

![Configuring Backend Security Group Information](/images/3-Workshop/3.2/3.2.2/Screenshot%202026-09-25%20211010.png)
![Configuring Backend Security Group Information](/images/3-Workshop/3.2/3.2.2/Screenshot%202026-09-25%20211035.png)

3. Under the **Inbound rules** section, click **Add rule**:
   - Type = `All TCP` (to support Dynamic Port Mapping for ECS on EC2).
   - Source = Select `Custom`, then type `sg-` in the search box and select `Eshop-ALB-SG` from the dropdown list.

![Adding Inbound Rule for Backend from ALB SG](/images/3-Workshop/3.2/3.2.2/Screenshot%202026-09-25%20211159.png)
![Adding Inbound Rule for Backend from ALB SG](/images/3-Workshop/3.2/3.2.2/Screenshot%202026-09-25%20211235.png)
![Adding Inbound Rule for Backend from ALB SG](/images/3-Workshop/3.2/3.2.2/Screenshot%202026-09-25%20211250.png)

4. Click **Create security group** to complete.

> **Security Note:** With this configuration, direct access using the internal IP of the EC2/Container is completely blocked. All incoming traffic is strictly required to pass through the Application Load Balancer (ALB).