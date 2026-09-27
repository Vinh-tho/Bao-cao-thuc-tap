---
title: "Internet Gateway and NAT Gateway"
weight: 2
pre: " <b> 3.1.2. </b> "
---

# 3.1.2. Configuring Internet Gateway (IGW) and NAT Gateway

This section outlines the process of configuring the Internet Gateway (IGW) and NAT Gateway to establish Internet connectivity for the Virtual Private Cloud (VPC). The Internet Gateway enables bidirectional communication between the Public Subnets and the Internet. Meanwhile, the NAT Gateway allows resources (such as Backend Containers) located in Private Subnets to establish outbound-only connections to the Internet (for downloading updates or libraries) without exposing their IP addresses to the public network.

### Step 1: Creating and Attaching the Internet Gateway (IGW)

1. On the **VPC** service console, select **Internet gateways** from the left navigation pane.
2. Click the **Create internet gateway** button.
3. In the *Name tag* field, enter `Eshop-IGW` for resource identification and click **Create internet gateway**.

![Creating Internet Gateway](/images/3-Workshop/3.1/3.1.2/Screenshot%202026-09-25%20185758.png)
![Creating Internet Gateway](/images/3-Workshop/3.1/3.1.2/Screenshot%202026-09-25%20185838.png)
![Creating Internet Gateway](/images/3-Workshop/3.1/3.1.2/Screenshot%202026-09-25%20185900.png)

4. Upon creation, the IGW status will be *Detached*. Click the **Actions** menu in the top right corner and select **Attach to VPC**.
5. Select `Eshop-VPC` (created in section 3.1.1) from the dropdown list and click **Attach internet gateway**.

![Attaching Internet Gateway to VPC](/images/3-Workshop/3.1/3.1.2/Screenshot%202026-09-25%20185954.png)
![Attaching Internet Gateway to VPC](/images/3-Workshop/3.1/3.1.2/Screenshot%202026-09-25%20190013.png)
![Attaching Internet Gateway to VPC](/images/3-Workshop/3.1/3.1.2/Screenshot%202026-09-25%20190042.png)

### Step 2: Creating the NAT Gateway

The NAT Gateway must be deployed in a **Public Subnet** and requires a static IP address (Elastic IP) to represent internal resources when routing traffic to the Internet.

1. In the left navigation pane, select **NAT gateways** and click the **Create NAT gateway** button.
2. Configure the following parameters:
   - **Name**: `Eshop-NAT-GW`
   - **Subnet**: Select `Eshop-Public-Subnet-1` (Deployment in a Public Subnet is mandatory).
   - **Connectivity type**: Select `Public`.
3. Under the **Elastic IP allocation ID** section, click the **Allocate Elastic IP** button to prompt AWS to automatically allocate a static public IP address for this NAT Gateway.
4. Scroll to the bottom of the page and click **Create NAT gateway** to execute.

![Creating NAT Gateway](/images/3-Workshop/3.1/3.1.2/Screenshot%202026-09-25%20190518.png)
![Creating NAT Gateway](/images/3-Workshop/3.1/3.1.2/Screenshot%202026-09-25%20190720.png)
![Creating NAT Gateway](/images/3-Workshop/3.1/3.1.2/Screenshot%202026-09-25%20190803.png)

*(Note: The NAT Gateway creation process may take a few minutes. The system status will transition from `Pending` to `Available` upon completion. Administrators can proceed to the next configuration steps while waiting).*