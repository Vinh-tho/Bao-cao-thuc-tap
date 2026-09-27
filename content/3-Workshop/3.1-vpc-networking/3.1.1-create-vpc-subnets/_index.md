---
title: "Creating VPC and Subnets"
weight: 1
pre: " <b> 3.1.1. </b> "
---

# 3.1.1. Creating the VPC, Public Subnets, and Private Subnets

This section outlines the process of creating a Virtual Private Cloud (VPC) to establish an isolated network environment for the E-shop system. This network space will then be divided into Public Subnets (used for deploying Load Balancers to receive internet traffic) and Private Subnets (ensuring security for Backend services by isolating them completely from the Internet).

### Step 1: Creating the VPC

1. Access the **VPC** service on the AWS Management Console.
2. In the left navigation pane, select **Your VPCs** and click the **Create VPC** button.
3. Configure the basic parameters for the VPC as follows:
   - **Resources to create**: Select `VPC only`.
   - **Name tag**: Enter `Eshop-VPC` to identify the resource.
   - **IPv4 CIDR block**: Select `IPv4 CIDR manual input` and specify the network range `10.0.0.0/16`.
4. Scroll to the bottom of the page and click **Create VPC** to complete the process.

![Creating VPC on AWS Console](/images/3-Workshop/3.1/3.1.1/Screenshot%202026-09-25%20181154.png)
![Creating VPC on AWS Console](/images/3-Workshop/3.1/3.1.1/Screenshot%202026-09-25%20181241.png)
![Creating VPC on AWS Console](/images/3-Workshop/3.1/3.1.1/Screenshot%202026-09-25%20181332.png)

### Step 2: Creating Subnets

The system requires a total of 4 Subnets (including 2 Public Subnets and 2 Private Subnets) distributed across 2 Availability Zones (AZs) to ensure High Availability for the network architecture.

1. In the VPC service navigation pane, select **Subnets** and click the **Create subnet** button.
2. In the **VPC ID** field, select the newly created `Eshop-VPC`.
3. Configure the parameters for **Public Subnet 1**:
   - **Subnet name**: `Eshop-Public-Subnet-1`
   - **Availability Zone**: `ap-southeast-1a`
   - **IPv4 CIDR block**: `10.0.1.0/24`

![Creating Public Subnet 1](/images/3-Workshop/3.1/3.1.1/Screenshot%202026-09-25%20182923.png)
![Creating Public Subnet 1](/images/3-Workshop/3.1/3.1.1/Screenshot%202026-09-25%20183054.png)
![Creating Public Subnet 1](/images/3-Workshop/3.1/3.1.1/Screenshot%202026-09-25%20183104.png)

4. Click **Add new subnet** to continue configuring **Public Subnet 2** in a different AZ:
   - **Subnet name**: `Eshop-Public-Subnet-2`
   - **Availability Zone**: `ap-southeast-1b`
   - **IPv4 CIDR block**: `10.0.2.0/24`

![Creating Public Subnet 2](/images/3-Workshop/3.1/3.1.1/Screenshot%202026-09-25%20183149.png)

5. Continue clicking **Add new subnet** to configure the 2 Private Subnets similarly:
   - **Private Subnet 1**: Subnet name = `Eshop-Private-Subnet-1`, Availability Zone = `ap-southeast-1a`, IPv4 CIDR block = `10.0.3.0/24`.
   - **Private Subnet 2**: Subnet name = `Eshop-Private-Subnet-2`, Availability Zone = `ap-southeast-1b`, IPv4 CIDR block = `10.0.4.0/24`.
6. After entering all the required information for the 4 Subnets, scroll down and click **Create subnet** to execute.

![Creating Private Subnets](/images/3-Workshop/3.1/3.1.1/Screenshot%202026-09-25%20183252.png)
![Creating Private Subnets](/images/3-Workshop/3.1/3.1.1/Screenshot%202026-09-25%20183318.png)
![Creating Private Subnets](/images/3-Workshop/3.1/3.1.1/Screenshot%202026-09-25%20183405.png)

### Step 3: Enabling auto-assign public IPv4 address for Public Subnets

To allow resources deployed in the Public Subnets (e.g., Application Load Balancer) to receive and communicate with internet traffic, these Subnets must be configured to automatically assign Public IPv4 addresses.

1. In the Subnets list, check the box next to `Eshop-Public-Subnet-1`.
2. In the top right corner, click the **Actions** menu and select **Edit subnet settings**.
3. Under the *Auto-assign IP settings* section, check the box for **Enable auto-assign public IPv4 address**.
4. Click **Save** to apply the configuration.

![Enabling Auto-assign public IP](/images/3-Workshop/3.1/3.1.1/Screenshot%202026-09-25%20184711.png)
![Enabling Auto-assign public IP](/images/3-Workshop/3.1/3.1.1/Screenshot%202026-09-25%20184734.png)
![Enabling Auto-assign public IP](/images/3-Workshop/3.1/3.1.1/Screenshot%202026-09-25%20184829.png)

*(Note: Repeat the procedures in Step 3 for `Eshop-Public-Subnet-2`)*.