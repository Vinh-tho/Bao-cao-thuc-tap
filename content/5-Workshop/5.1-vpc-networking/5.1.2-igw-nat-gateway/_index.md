---
title: "Internet Gateway and NAT Gateway"
weight: 2
pre: " <b> 5.1.2. </b> "
---

# 5.1.2. Configuring Internet Gateway (IGW) and NAT Gateway

For our Virtual Private Cloud (VPC) to communicate with the external Internet, we need to set up an **Internet Gateway (IGW)** (allowing two-way internet access for Public Subnets) and a **NAT Gateway** (allowing the Container Backend resources in Private Subnets to have one-way outbound internet access to download updates/libraries without being directly exposed).

### Step 1: Create and Attach an Internet Gateway (IGW)

1. In the **VPC** service console, select **Internet gateways** from the left navigation pane.
2. Click the **Create internet gateway** button.
3. For the *Name tag*, enter `Eshop-IGW` and click the **Create internet gateway** button.

![Tạo Internet Gateway](/Bao-cao-thuc-tap/images/5-Workshop/5.1/5.1.2/Screenshot%202026-09-25%20185758.png)
![Tạo Internet Gateway](/Bao-cao-thuc-tap/images/5-Workshop/5.1/5.1.2/Screenshot%202026-09-25%20185838.png)
![Tạo Internet Gateway](/Bao-cao-thuc-tap/images/5-Workshop/5.1/5.1.2/Screenshot%202026-09-25%20185900.png)

4. Once created, its state will be *Detached*. Click the **Actions** button in the top right corner and choose **Attach to VPC**.
5. Select `Eshop-VPC` (created in section 5.1.1) from the dropdown list and click **Attach internet gateway**.

![Gắn Internet Gateway vào VPC](/Bao-cao-thuc-tap/images/5-Workshop/5.1/5.1.2/Screenshot%202026-09-25%20185954.png)
![Gắn Internet Gateway vào VPC](/Bao-cao-thuc-tap/images/5-Workshop/5.1/5.1.2/Screenshot%202026-09-25%20190013.png)
![Gắn Internet Gateway vào VPC](/Bao-cao-thuc-tap/images/5-Workshop/5.1/5.1.2/Screenshot%202026-09-25%20190042.png)

### Step 2: Create a NAT Gateway

The NAT Gateway must be placed in a **Public Subnet** and requires a static IP address (Elastic IP) to represent internal servers when they route traffic to the Internet.

1. In the left menu, select **NAT gateways** and click the **Create NAT gateway** button.
2. Fill in the following details:
   - **Name**: `Eshop-NAT-GW`
   - **Subnet**: Select `Eshop-Public-Subnet-1` (You must select a Public Subnet).
   - **Connectivity type**: Select `Public`.
3. Under **Elastic IP allocation ID**, click the **Allocate Elastic IP** button to have AWS automatically assign a static public IP address to this NAT Gateway.
4. Scroll down to the bottom and click **Create NAT gateway**.

![Khởi tạo NAT Gateway](/Bao-cao-thuc-tap/images/5-Workshop/5.1/5.1.2/Screenshot%202026-09-25%20190518.png)
![Khởi tạo NAT Gateway](/Bao-cao-thuc-tap/images/5-Workshop/5.1/5.1.2/Screenshot%202026-09-25%20190720.png)
![Khởi tạo NAT Gateway](/Bao-cao-thuc-tap/images/5-Workshop/5.1/5.1.2/Screenshot%202026-09-25%20190803.png)

*(Note: The NAT Gateway creation process may take a few minutes. Its state will change from `Pending` to `Available` when the process is complete. You can proceed to the next section while waiting).*