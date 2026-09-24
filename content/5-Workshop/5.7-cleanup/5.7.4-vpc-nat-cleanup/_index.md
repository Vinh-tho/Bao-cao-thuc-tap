---
title: "Cleanup VPC & NAT Gateway"
date: 2026-09-24
weight: 4
chapter: false
pre: " <b> 5.7.4. </b> "
---

# 5.7.4. Cleaning up VPC, NAT Gateway & Elastic IP (Avoid unexpected costs)

This is the most critical step in the cleanup process because **NAT Gateways and Elastic IPs incur hourly charges**, even if they are sitting idle. You must delete the NAT Gateway first, then release its IP, and only then can you delete the VPC.

### Step 1: Delete NAT Gateway

1. Navigate to the **VPC** service on the AWS Console.
2. In the left menu, select **NAT Gateways**.
3. Check the box for your E-shop project's NAT Gateway (e.g., `Eshop-NAT-Gateway`).
4. Click **Actions** -> **Delete NAT gateway**.
5. Type `delete` in the confirmation box and click **Delete**.

> **Crucial Note:** The NAT Gateway's status will change to *Deleting*. You **MUST** wait about 3 to 5 minutes until the status completely changes to *Deleted* before proceeding to Step 2. Otherwise, AWS will throw an error stating the IP is still in use.

![Delete NAT Gateway](/images/5-Workshop/5.7.4/delete_nat_gateway.png)

### Step 2: Release Elastic IP (EIP)

1. Still in the VPC console, scroll down the left menu to the **Virtual private cloud** section and select **Elastic IPs**.
2. Check the box next to the static IP address that was allocated to your NAT Gateway.
3. Click **Actions** -> **Release Elastic IP addresses**.
4. Confirm by clicking **Release**. (This returns the IP to AWS's pool, ensuring you aren't charged for holding an unused static IP).

![Release Elastic IP](/images/5-Workshop/5.7.4/release_elastic_ip.png)

### Step 3: Delete VPC (Mass Cleanup)

AWS has a very smart feature: When you delete a VPC, it automatically cleans up and deletes all associated Subnets, Route Tables, Internet Gateways, and Security Groups inside it.

1. In the left menu, select **Your VPCs**.
2. Check the box for `Eshop-VPC`.
3. Click **Actions** -> **Delete VPC**.
4. A confirmation panel will list all associated resources that are about to be deleted with it. Review the list, type `delete`, and click **Delete**.

![Delete Entire VPC and Associated Resources](/images/5-Workshop/5.7.4/delete_vpc.png)

You have now completed the resource cleanup process. Your AWS account is back to a safe state with no ongoing charges from this project.