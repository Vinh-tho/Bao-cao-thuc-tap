---
title: "Building the Networking Foundation"
date: 2026-09-24
weight: 1
chapter: false
pre: " <b> 5.1. </b> "
---

# 5.1. Building the Networking Foundation with Amazon VPC

In this chapter, we will start building the networking infrastructure foundation for the Web E-shop system. Setting up **Amazon VPC** according to Best Practices with **Public Subnets** (for the Application Load Balancer) and **Private Subnets** (for the EC2/ECS Backend) is the most critical step to ensure security and High Availability for the entire system.

### Prerequisites:
1.  **AWS Account** with Administrator Access, or an IAM User with full permissions to operate on VPC, EC2, ECS, S3, Lambda, and IAM.
2.  **Web Browser** (Google Chrome, Firefox, Safari, or Microsoft Edge).

---

### Step 1: Log in to the Console and Switch Region
To ensure consistency throughout the workshop, we will uniformly use the Region closest to users in Vietnam.

1. Access the [AWS Management Console](https://console.aws.amazon.com/) and log in to your account.
2. In the top right corner of the navigation bar, select the **Asia Pacific (Singapore) - ap-southeast-1** Region.

![Switch Region to Singapore](/images/5-Workshop/img_A/image1.png)

---

### Overview of the upcoming practical sections:

The E-shop's network architecture will be deployed with:
- **1 VPC** (Virtual Private Cloud) to define a private network space.
- **2 Public Subnets** across 2 different Availability Zones (for direct Internet connectivity via an Internet Gateway, used by the ALB).
- **2 Private Subnets** across 2 different Availability Zones (completely isolated from the Internet, allowing one-way outbound Internet access via a NAT Gateway, used for running the Container Backend).

Please follow the detailed practical exercises below sequentially:

* [5.1.1. Creating a VPC, Public Subnets, and Private Subnets](5.1.1-create-vpc-subnets/)
* [5.1.2. Configuring Internet Gateway (IGW) and NAT Gateway](5.1.2-igw-nat-gateway/)
* [5.1.3. Configuring Route Tables for Network Traffic](5.1.3-route-tables/)