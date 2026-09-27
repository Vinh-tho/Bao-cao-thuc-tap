---
title: "Building the Networking Foundation"
date: 2026-09-24
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---

# 3.1. Building the Networking Foundation with Amazon VPC

This chapter outlines the process of building the foundational network infrastructure for the E-shop Web system. Establishing an **Amazon VPC** in accordance with AWS Best Practices—utilizing **Public Subnets** (for the Application Load Balancer) and **Private Subnets** (for Backend EC2/ECS services)—is a critical step to ensure security and High Availability for the entire system.

### Prerequisites:

1. An **AWS Account** with Administrator Access, or an IAM User with full permissions to manage VPC, EC2, ECS, S3, Lambda, and IAM services.
2. A **Web browser** (Google Chrome, Firefox, Safari, or Microsoft Edge).

---

### Step 1: Logging into the Console and Switching Regions

To maintain consistency throughout the deployment process, the infrastructure will be configured in a Region geographically closest to end-users in Vietnam.

1. Access the [AWS Management Console](https://console.aws.amazon.com/) and log in to the account.
2. In the top right corner of the navigation bar, select the **Asia Pacific (Singapore) - ap-southeast-1** Region.

![Switching Region to Singapore](/images/3-Workshop/3.1/chuyen%20vung.png)

---

### Network Architecture Overview:

The E-shop network architecture is designed and deployed with the following components:

- **1 VPC** (Virtual Private Cloud) to define the isolated network space.
- **2 Public Subnets** distributed across 2 different Availability Zones (enabling direct Internet connectivity via an Internet Gateway, used for the ALB).
- **2 Private Subnets** distributed across 2 different Availability Zones (completely isolated from the Internet, with outbound-only Internet access via a NAT Gateway, used for deploying Backend Containers).

The detailed deployment procedures are presented in the following sections:

- [3.1.1. Creating the VPC, Public Subnets, and Private Subnets](3.1.1-create-vpc-subnets/)
- [3.1.2. Configuring Internet Gateway (IGW) and NAT Gateway](3.1.2-igw-nat-gateway/)
- [3.1.3. Configuring Route Tables for Network Traffic](3.1.3-route-tables/)