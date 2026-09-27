---
title: "Cleaning Up VPC & NAT Gateway"
date: 2026-09-24
weight: 4
chapter: false
pre: " <b> 3.7.4. </b> "
---

# 3.7.4. Cleaning Up VPC, NAT Gateway & Elastic IP (Preventing Unexpected Costs)

This is a critically important step in the overall cleanup process, as **NAT Gateway and Elastic IP are services billed on an hourly basis**, regardless of active utilization. The technical procedure mandates deleting the NAT Gateway first, followed by releasing the Elastic IP, and ultimately deleting the VPC.

### Step 1: Deleting the NAT Gateway

1. Access the **VPC** service on the AWS Management Console.
2. In the left navigation pane, select **NAT Gateways**.
3. Check the NAT Gateway associated with the E-shop project (e.g., `Eshop-NAT-Gateway`).
4. Click the **Actions** menu and select **Delete NAT gateway**.
5. Enter the keyword `delete` into the confirmation box and click **Delete** to execute.

> **Critical Note:** The NAT Gateway status will transition to *Deleting*. It is **MANDATORY** to wait approximately 3 to 5 minutes until the status fully changes to *Deleted* before proceeding to Step 2. If attempted prematurely, AWS will return an error indicating that the IP address is currently in use.

### Step 2: Releasing the Elastic IP (EIP)

1. Within the VPC console, scroll down the left navigation pane to the **Virtual private cloud** section and select **Elastic IPs**.
2. Check the static IP address previously allocated to the NAT Gateway.
3. Click the **Actions** menu and select **Release Elastic IP addresses**.
4. Confirm by clicking **Release**. (This operation returns the IP address to the AWS resource pool, preventing ongoing charges for static IP retention).

### Step 3: Deleting the VPC (Automated Bulk Cleanup)

AWS provides an automated cleanup mechanism: When deleting a VPC, the system automatically removes all associated dependent resources contained within it, including Subnets, Route Tables, Internet Gateways, and Security Groups.

1. In the left navigation pane, select **Your VPCs**.
2. Check `Eshop-VPC`.
3. Click the **Actions** menu and select **Delete VPC**.
4. A confirmation prompt will list all dependent resources slated for deletion. Review the list, enter the keyword `delete`, and click the **Delete** button.

*(Note: The resource cleanup process is now fully complete. The AWS account environment has been restored to its initial safe state, ensuring no further charges will be incurred in relation to this deployment project).*