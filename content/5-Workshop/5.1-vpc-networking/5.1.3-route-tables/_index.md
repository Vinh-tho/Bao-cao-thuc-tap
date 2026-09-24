---
title: "Configuring Route Tables"
weight: 3
pre: " <b> 5.1.3. </b> "
---

# 5.1.3. Configuring Route Tables for Network Traffic

A **Route Table** acts as a set of rules (like signposts) that determine where network traffic from your subnets or gateways is directed. We need to create 2 Route Tables: one for the **Public Subnets** (routing to the Internet Gateway) and one for the **Private Subnets** (routing to the NAT Gateway).

### Step 1: Create and Configure the Public Route Table

This Route Table allows resources (like the Load Balancer) to connect directly to the Internet.

1. In the **VPC** console, select **Route tables** from the left menu.
2. Click the **Create route table** button.
3. Fill in the details:
   - **Name**: `Eshop-Public-RT`
   - **VPC**: Select `Eshop-VPC`
4. Click **Create route table**.

![Create Public Route Table](/images/5-Workshop/5.1.3/create_public_rt.png)

5. Once created, in the details page of `Eshop-Public-RT`, go to the **Routes** tab at the bottom and click **Edit routes**.
6. Click **Add route**:
   - **Destination**: Enter `0.0.0.0/0` (Represents all internet traffic).
   - **Target**: Select **Internet Gateway**, then choose `Eshop-IGW` created in the previous section.
7. Click **Save changes**.

![Add Route to Internet Gateway](/images/5-Workshop/5.1.3/edit_public_routes.png)

8. Switch to the **Subnet associations** tab and click **Edit subnet associations**.
9. Check the boxes for your 2 Public Subnets (`Eshop-Public-Subnet-1` and `Eshop-Public-Subnet-2`), then click **Save associations**.

![Associate Public Subnets](/images/5-Workshop/5.1.3/associate_public_subnets.png)

### Step 2: Create and Configure the Private Route Table

This Route Table ensures the Backend (EC2/ECS) remains isolated from the public Internet, while still allowing outbound traffic through the NAT Gateway for necessary updates.

1. Similarly, click **Create route table**.
2. Fill in the details:
   - **Name**: `Eshop-Private-RT`
   - **VPC**: Select `Eshop-VPC`
3. Click **Create route table**.

![Create Private Route Table](/images/5-Workshop/5.1.3/create_private_rt.png)

4. Open the details for `Eshop-Private-RT`, go to the **Routes** tab, and click **Edit routes**.
5. Click **Add route**:
   - **Destination**: Enter `0.0.0.0/0`.
   - **Target**: Select **NAT Gateway**, then choose `Eshop-NAT-GW`.
6. Click **Save changes**.

![Add Route to NAT Gateway](/images/5-Workshop/5.1.3/edit_private_routes.png)

7. Switch to the **Subnet associations** tab and click **Edit subnet associations**.
8. Check the boxes for your 2 Private Subnets (`Eshop-Private-Subnet-1` and `Eshop-Private-Subnet-2`), then click **Save associations**.

![Associate Private Subnets](/images/5-Workshop/5.1.3/associate_private_subnets.png)

*🎉 **Congratulations!** You have successfully set up the fundamental networking infrastructure (VPC, Subnets, Gateways, Route Tables) in a highly secure and AWS Best Practice compliant manner for the E-shop project.*