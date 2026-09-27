---
title: "Configuring Route Tables"
weight: 3
pre: " <b> 3.1.3. </b> "
---

# 3.1.3. Configuring Route Tables for Network Traffic

This section outlines the configuration of Route Tables to direct network traffic within the VPC. The architecture requires the deployment of two Route Tables: one for the **Public Subnets** (routing traffic to the Internet Gateway) and one for the **Private Subnets** (routing traffic to the NAT Gateway).

### Step 1: Creating and Configuring the Public Route Table

This Route Table enables resources (such as Load Balancers) to establish direct connections to the Internet.

1. On the **VPC** service console, select **Route tables** from the left navigation pane.
2. Click the **Create route table** button.
3. Configure the following parameters:
   - **Name**: `Eshop-Public-RT`
   - **VPC**: Select `Eshop-VPC`
4. Click **Create route table** to execute.

![Creating Public Route Table](/images/3-Workshop/3.1/3.1.3/Screenshot%202026-09-25%20191413.png)
![Creating Public Route Table](/images/3-Workshop/3.1/3.1.3/Screenshot%202026-09-25%20191439.png)
![Creating Public Route Table](/images/3-Workshop/3.1/3.1.3/Screenshot%202026-09-25%20191514.png)

5. Upon successful creation, access the details page of `Eshop-Public-RT`, select the **Routes** tab in the lower section, and click **Edit routes**.
6. Click **Add route** and enter the routing information:
   - **Destination**: Enter `0.0.0.0/0` (Representing all Internet IP addresses).
   - **Target**: Select **Internet Gateway**, then choose the previously created `Eshop-IGW`.
7. Click **Save changes** to apply the configuration.

![Adding Route to Internet Gateway](/images/3-Workshop/3.1/3.1.3/Screenshot%202026-09-25%20191714.png)
![Adding Route to Internet Gateway](/images/3-Workshop/3.1/3.1.3/Screenshot%202026-09-25%20191812.png)
![Adding Route to Internet Gateway](/images/3-Workshop/3.1/3.1.3/Screenshot%202026-09-25%20191828.png)

8. Switch to the **Subnet associations** tab and click **Edit subnet associations**.
9. Check the boxes for the 2 Public Subnets (`Eshop-Public-Subnet-1` and `Eshop-Public-Subnet-2`), then click **Save associations** to complete the linkage.

![Associating Public Subnets](/images/3-Workshop/3.1/3.1.3/Screenshot%202026-09-25%20191854.png)
![Associating Public Subnets](/images/3-Workshop/3.1/3.1.3/Screenshot%202026-09-25%20191918.png)
![Associating Public Subnets](/images/3-Workshop/3.1/3.1.3/Screenshot%202026-09-25%20191936.png)

### Step 2: Creating and Configuring the Private Route Table

This Route Table ensures that Backend services (EC2/ECS) remain securely isolated from the public Internet while allowing traffic to be routed through the NAT Gateway for downloading necessary updates or libraries.

1. Following the same procedure as in Step 1, click **Create route table**.
2. Configure the following parameters:
   - **Name**: `Eshop-Private-RT`
   - **VPC**: Select `Eshop-VPC`
3. Click **Create route table** to execute.

![Creating Private Route Table](/images/3-Workshop/3.1/3.1.3/Screenshot%202026-09-25%20192654.png)
![Creating Private Route Table](/images/3-Workshop/3.1/3.1.3/Screenshot%202026-09-25%20192703.png)

4. Access the details page of `Eshop-Private-RT`, select the **Routes** tab, and click **Edit routes**.
5. Click **Add route** and enter the routing information:
   - **Destination**: Enter `0.0.0.0/0`.
   - **Target**: Select **NAT Gateway**, then choose `Eshop-NAT-GW`.
6. Click **Save changes** to apply the configuration.

![Adding Route to NAT Gateway](/images/3-Workshop/3.1/3.1.3/Screenshot%202026-09-25%20192718.png)
![Adding Route to NAT Gateway](/images/3-Workshop/3.1/3.1.3/Screenshot%202026-09-25%20192752.png)
![Adding Route to NAT Gateway](/images/3-Workshop/3.1/3.1.3/Screenshot%202026-09-25%20192810.png)

7. Switch to the **Subnet associations** tab and click **Edit subnet associations**.
8. Check the boxes for the 2 Private Subnets (`Eshop-Private-Subnet-1` and `Eshop-Private-Subnet-2`), then click **Save associations** to complete the linkage.

![Associating Private Subnets](/images/3-Workshop/3.1/3.1.3/Screenshot%202026-09-25%20192837.png)
![Associating Private Subnets](/images/3-Workshop/3.1/3.1.3/Screenshot%202026-09-25%20192854.png)
![Associating Private Subnets](/images/3-Workshop/3.1/3.1.3/Screenshot%202026-09-25%20192909.png)

*(Note: The setup of the foundational network infrastructure, including VPC, Subnets, Gateways, and Route Tables, is now complete. This architecture ensures security and complies with AWS Best Practices for the E-shop system).*