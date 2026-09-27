---
title: "Cleaning Up ECS & ECR"
date: 2026-09-24
weight: 2
chapter: false
pre: " <b> 3.7.2. </b> "
---

# 3.7.2. Cleaning Up ECS Cluster, Task Definitions & Amazon ECR

After the virtual servers (EC2 instances) have been automatically terminated by the Auto Scaling Group (ASG), the next procedure requires removing the logical configurations for managing Containers on ECS and the Docker Image repository on ECR.

### Step 1: Deleting the ECS Service and Cluster

1. Access the **ECS (Elastic Container Service)** on the AWS Management Console.
2. In the left navigation pane, select **Clusters** and click on `Eshop-ECS-Cluster`.
3. Under the **Services** tab, select `Eshop-Backend-Service` and click the **Delete** button. Enter the keyword `delete` to confirm. The Service deletion process will take some time as the system drains the running tasks.
4. Once the Service has been completely removed from the list, click the **Delete cluster** button in the top right corner. Enter the cluster name `Eshop-ECS-Cluster` to confirm and click **Delete**.

### Step 2: Deregistering Task Definitions

1. In the left navigation pane of the ECS console, select **Task definitions**.
2. Select `Eshop-Backend-Task`.
3. Check all revisions that are currently in the *Active* state.
4. Click the **Actions** menu and select **Deregister**. (Note: AWS does not support the immediate permanent deletion of Task Definitions; these records are transitioned to an Inactive state for historical auditing purposes).

### Step 3: Deleting the Amazon ECR Repository

1. Access the **Amazon ECR** (Elastic Container Registry) service.
2. In the left navigation pane, select **Repositories**.
3. Check the repository named `eshop-backend`.
4. Click the **Delete** button in the top right corner.
5. Enter the keyword `delete` into the confirmation box and click **Delete** to execute.

*(Note: The resource cleanup process for the Backend Container subsystem is now complete. The following section will guide the removal of Serverless services, including the AWS Lambda function and the S3 storage system).*