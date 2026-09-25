---
title: "Creating VPC and Subnets"
weight: 1
pre: " <b> 5.1.1. </b> "
---

# 5.1.1. Creating a VPC, Public Subnets, and Private Subnets

In this lab, we will create a Virtual Private Cloud (VPC) to serve as the private network space for our E-shop. We will then divide this space into Public Subnets (to host the Load Balancer that receives internet traffic) and Private Subnets (to run the Backend securely, isolated from the internet).

### Step 1: Create a VPC

1. Navigate to the **VPC** service in the AWS Management Console.
2. In the left navigation pane, choose **Your VPCs** and click the **Create VPC** button.
3. Configure the basic settings for your VPC:
   - **Resources to create**: Select `VPC only`.
   - **Name tag**: Enter `Eshop-VPC` for easy identification.
   - **IPv4 CIDR block**: Select `IPv4 CIDR manual input` and enter `10.0.0.0/16`.
4. Scroll to the bottom of the page and click **Create VPC**.

![Create VPC on AWS Console](/Bao-cao-thuc-tap/images/5-Workshop/5.1/5.1.1/Screenshot%202026-09-25%20181154.png)
![Create VPC on AWS Console](/Bao-cao-thuc-tap/images/5-Workshop/5.1/5.1.1/Screenshot%202026-09-25%20181241.png)
![Create VPC on AWS Console](/Bao-cao-thuc-tap/images/5-Workshop/5.1/5.1.1/Screenshot%202026-09-25%20181332.png)

### Step 2: Create the Subnets

We will create a total of 4 Subnets (2 Public and 2 Private) spanning across 2 Availability Zones (AZs) to ensure High Availability.

1. In the left menu of the VPC console, choose **Subnets** and click **Create subnet**.
2. Under **VPC ID**, select the `Eshop-VPC` we just created.
3. Fill in the details for **Public Subnet 1**:
   - **Subnet name**: `Eshop-Public-Subnet-1`
   - **Availability Zone**: `ap-southeast-1a`
   - **IPv4 CIDR block**: `10.0.1.0/24`

![Create Public Subnet 1](/Bao-cao-thuc-tap/images/5-Workshop/5.1/5.1.1/Screenshot%202026-09-25%20182923.png)
![Create Public Subnet 1](/Bao-cao-thuc-tap/images/5-Workshop/5.1/5.1.1/Screenshot%202026-09-25%20183054.png)
![Create Public Subnet 1](/Bao-cao-thuc-tap/images/5-Workshop/5.1/5.1.1/Screenshot%202026-09-25%20183104.png)

4. Click the **Add new subnet** button to create **Public Subnet 2** in a different AZ:
   - **Subnet name**: `Eshop-Public-Subnet-2`
   - **Availability Zone**: `ap-southeast-1b`
   - **IPv4 CIDR block**: `10.0.2.0/24`

![Create Public Subnet 2](/Bao-cao-thuc-tap/images/5-Workshop/5.1/5.1.1/Screenshot%202026-09-25%20183149.png)

1. Continue clicking **Add new subnet** to create the 2 Private Subnets similarly:
   - **Private Subnet 1**: Name = `Eshop-Private-Subnet-1`, AZ = `ap-southeast-1a`, CIDR = `10.0.3.0/24`.
   - **Private Subnet 2**: Name = `Eshop-Private-Subnet-2`, AZ = `ap-southeast-1b`, CIDR = `10.0.4.0/24`.
2. Once all 4 Subnets are defined, scroll down and click **Create subnet**.

![Create Private Subnets](/Bao-cao-thuc-tap/images/5-Workshop/5.1/5.1.1/Screenshot%202026-09-25%20183252.png)
![Create Private Subnets](/Bao-cao-thuc-tap/images/5-Workshop/5.1/5.1.1/Screenshot%202026-09-25%20183318.png)
![Create Private Subnets](/Bao-cao-thuc-tap/images/5-Workshop/5.1/5.1.1/Screenshot%202026-09-25%20183405.png)

### Step 3: Enable Auto-assign Public IP for Public Subnets

For resources placed in the Public Subnets (like the Application Load Balancer later) to communicate externally, the subnet needs to automatically assign Public IPv4 addresses.

1. In the Subnets list, check the box next to `Eshop-Public-Subnet-1`.
2. Click the **Actions** button in the top right corner and choose **Edit subnet settings**.
3. Under *Auto-assign IP settings*, check the box for **Enable auto-assign public IPv4 address**.
4. Click **Save**.

![Bật Auto-assign public IP](/Bao-cao-thuc-tap/images/5-Workshop/5.1/5.1.1/Screenshot%202026-09-25%20184711.png)
![Bật Auto-assign public IP](/Bao-cao-thuc-tap/images/5-Workshop/5.1/5.1.1/Screenshot%202026-09-25%20184734.png)
![Bật Auto-assign public IP](/Bao-cao-thuc-tap/images/5-Workshop/5.1/5.1.1/Screenshot%202026-09-25%20184829.png)

*(Note: Repeat Step 3 for `Eshop-Public-Subnet-2` as well)*.