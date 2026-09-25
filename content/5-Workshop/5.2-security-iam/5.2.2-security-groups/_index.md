---
title: "Creating Security Groups"
date: 2026-09-24
weight: 2
chapter: false
pre: " <b> 5.2.2. </b> "
---

# 5.2.2. Creating Security Groups for ALB and ECS Backend

A **Security Group (SG)** acts as a virtual firewall at the instance level to control inbound and outbound network traffic. To adhere to maximum security principles, we will create 2 SGs:
1. **ALB Security Group**: Open to the Internet to serve web traffic.
2. **Backend Security Group**: Completely hidden from the Internet, allowing inbound traffic *only* from the ALB SG.

### Step 1: Create a Security Group for the Application Load Balancer (ALB)

1. In the AWS Console, go to the **VPC** (or EC2) service, look at the left navigation pane, scroll down to *Security*, and select **Security Groups**.
2. Click the **Create security group** button.
3. Fill in the basic details:
   - **Security group name**: `Eshop-ALB-SG`
   - **Description**: `Allow HTTP and HTTPS traffic from Internet to ALB`
   - **VPC**: Click the `X` to remove the default VPC, then select our `Eshop-VPC`.

![Cấu hình thông tin ALB Security Group](/images/5-Workshop/5.2/5.2.2/Screenshot%202026-09-25%20210249.png)
![Cấu hình thông tin ALB Security Group](/images/5-Workshop/5.2/5.2.2/Screenshot%202026-09-25%20210335.png)

4. Under **Inbound rules**, click **Add rule** twice to open web ports:
   - Rule 1: Type = `HTTP`, Source = `Anywhere-IPv4` (`0.0.0.0/0`)
   - Rule 2: Type = `HTTPS`, Source = `Anywhere-IPv4` (`0.0.0.0/0`)
5. Leave the **Outbound rules** as default (Allow All traffic) and click the **Create security group** button at the bottom.

![Thêm Inbound rules cho ALB](/images/5-Workshop/5.2/5.2.2/Screenshot%202026-09-25%20210441.png)
![Thêm Inbound rules cho ALB](/images/5-Workshop/5.2/5.2.2/Screenshot%202026-09-25%20210507.png)
![Thêm Inbound rules cho ALB](/images/5-Workshop/5.2/5.2.2/Screenshot%202026-09-25%20210519.png)

### Step 2: Create a Security Group for the Backend (EC2 / ECS Task)

Now we create the SG for the Backend. The critical point here is that the Backend will NOT open ports to the public Internet (`0.0.0.0/0`). Instead, it will use the ALB's SG as the Source.

1. Return to the Security Groups list and click **Create security group** again.
2. Fill in the basic details:
   - **Security group name**: `Eshop-Backend-SG`
   - **Description**: `Allow traffic only from ALB`
   - **VPC**: Select `Eshop-VPC`.

![Cấu hình thông tin Backend Security Group](/images/5-Workshop/5.2/5.2.2/Screenshot%202026-09-25%20211010.png)
![Cấu hình thông tin Backend Security Group](/images/5-Workshop/5.2/5.2.2/Screenshot%202026-09-25%20211035.png)

3. Under **Inbound rules**, click **Add rule**:
   - Type = `All TCP` (to support ECS Dynamic Port Mapping on EC2).
   - Source = Select `Custom`, then type `sg-` in the search box and select `Eshop-ALB-SG` from the dropdown list.

![Thêm Inbound rule cho Backend từ ALB SG](/images/5-Workshop/5.2/5.2.2/Screenshot%202026-09-25%20211159.png)
![Thêm Inbound rule cho Backend từ ALB SG](/images/5-Workshop/5.2/5.2.2/Screenshot%202026-09-25%20211235.png)
![Thêm Inbound rule cho Backend từ ALB SG](/images/5-Workshop/5.2/5.2.2/Screenshot%202026-09-25%20211250.png)

4. Click **Create security group**.

> **Security Note:** With this setup, even if someone discovers the internal IP of the EC2/Container, they cannot access it directly. The request must pass through the Load Balancer (ALB) gateway.