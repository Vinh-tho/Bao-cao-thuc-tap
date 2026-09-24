---
title: "Cleanup ECS & ECR"
date: 2026-09-24
weight: 2
chapter: false
pre: " <b> 5.7.2. </b> "
---

# 5.7.2. Cleaning up ECS Cluster, Task Definitions & Amazon ECR

Now that the physical servers (EC2) have been terminated by the ASG, we need to delete the logical configurations managing your Containers in ECS and the Docker Image registry.

### Step 1: Delete ECS Service and Cluster

1. Navigate to the **ECS (Elastic Container Service)** on the AWS Console.
2. In the left menu, select **Clusters** and click on `Eshop-ECS-Cluster`.
3. Under the **Services** tab, select `Eshop-Backend-Service` and click the **Delete** button. Type `delete` to confirm. It may take a few moments for the service to drain its tasks and fully delete.
4. Once the Service disappears from the list, click the **Delete cluster** button in the top right corner. Type the cluster name `Eshop-ECS-Cluster` to confirm and click **Delete**.

![Delete ECS Service and Cluster](/images/5-Workshop/5.7.2/delete_ecs_cluster.png)

### Step 2: Deregister Task Definitions

1. In the left menu of the ECS console, select **Task definitions**.
2. Select `Eshop-Backend-Task`.
3. Check the boxes for all revisions currently in the *Active* status.
4. Click **Actions** -> **Deregister**. (AWS does not allow permanent immediate deletion of Task Definitions; they are marked as Inactive to preserve historical records).

![Deregister Task Definition](/images/5-Workshop/5.7.2/deregister_task_def.png)

### Step 3: Delete Amazon ECR Repository

1. Navigate to the **Amazon ECR** (Elastic Container Registry) service.
2. In the left menu, select **Repositories**.
3. Check the box next to the `eshop-backend` repository (which holds your Docker Image).
4. Click the **Delete** button in the top right corner.
5. Type `delete` in the confirmation box and click **Delete**.

![Delete ECR Repository](/images/5-Workshop/5.7.2/delete_ecr_repo.png)

Your Backend cluster is now completely dismantled. Next, we will clean up the Serverless services, including the image-resizing Lambda and S3.