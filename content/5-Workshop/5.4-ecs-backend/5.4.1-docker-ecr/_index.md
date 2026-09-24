---
title: "Build & Push Image to ECR"
date: 2026-09-24
weight: 1
chapter: false
pre: " <b> 5.4.1. </b> "
---

# 5.4.1. Packaging the Backend (Dockerfile) & Pushing the Image to Amazon ECR

To run our Backend application (handling cart, payments, etc.) on the ECS cluster, we need to package the source code into a **Docker Image** and store it on **Amazon ECR (Elastic Container Registry)** — AWS's secure container image registry.

*(Note: To complete this exercise, your local machine must have **Docker Desktop** and the **AWS CLI** configured).*

### Step 1: Create a Repository on Amazon ECR

1. Access the AWS Console, search for **ECR**, and select **Elastic Container Registry**.
2. In the left menu, select **Repositories**, then click **Create repository**.
3. Under **Visibility settings**, choose **Private** (Only internal AWS services can pull this Image).
4. For **Repository name**, enter `eshop-backend`.
5. Scroll to the bottom and click **Create repository**.

![Create ECR Repository](/images/5-Workshop/5.4.1/create_ecr_repo.png)

### Step 2: View Push Commands

1. In the Repositories list, check the box next to the newly created `eshop-backend`.
2. Click the **View push commands** button in the top right corner. AWS will provide 4 ready-to-use commands for your Terminal/Command Prompt.

![View Push Commands](/images/5-Workshop/5.4.1/view_push_commands.png)

### Step 3: Build and Push the Image to ECR

Open your Terminal (or CMD/PowerShell) in the E-shop's Backend source code directory (where the `Dockerfile` is located). Run the 4 commands provided by AWS in Step 2 sequentially:

1. **Authenticate Docker to your Amazon ECR registry:**
   ```bash
   aws ecr get-login-password --region ap-southeast-1 | docker login --username AWS --password-stdin <AWS_ACCOUNT_ID>.dkr.ecr.ap-southeast-1.amazonaws.com
   ```

2. **Build your Docker Image from source:**
   ```bash
   docker build -t eshop-backend .
   ```

3. **Tag your newly built Image:**
   ```bash
   docker tag eshop-backend:latest <AWS_ACCOUNT_ID>[.dkr.ecr.ap-southeast-1.amazonaws.com/eshop-backend:latest](https://.dkr.ecr.ap-southeast-1.amazonaws.com/eshop-backend:latest)
   ```

4. **Push the Image to Amazon ECR:**
   ```bash
   docker push <AWS_ACCOUNT_ID>[.dkr.ecr.ap-southeast-1.amazonaws.com/eshop-backend:latest](https://.dkr.ecr.ap-southeast-1.amazonaws.com/eshop-backend:latest)
   ```

Once the Push command reaches 100%, return to the ECR interface on the AWS Console and click on the `eshop-backend` repository name. You will see the Image tagged `latest` securely stored in the registry, fully ready for the ECS system to pull and deploy!