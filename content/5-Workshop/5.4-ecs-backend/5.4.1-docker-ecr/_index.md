---
title: "Packaging & Pushing the Image to ECR"
date: 2026-09-24
weight: 1
chapter: false
pre: " <b> 5.4.1. </b> "
---

# 5.4.1. Packaging the Backend (Dockerfile) & Pushing the Image to Amazon ECR

To deploy the Backend application (the business logic API) on the ECS cluster, the source code needs to be packaged into a Docker Image and stored centrally on Amazon ECR (Elastic Container Registry) — AWS's secure container image storage service.

_(Prerequisites: The local deployment environment must have Docker Desktop installed and AWS CLI authentication configured)._

### Step 1: Create a Repository on Amazon ECR

1. Access the AWS Console interface, search for and navigate to the **Elastic Container Registry (ECR)** service.
2. In the left navigation bar, under **Private registry**, select **Repositories**. Then, click the orange **Create repository** button in the top-right corner of the screen.
3. On the **Create private repository** interface, go to the **General settings** section and enter `eshop-backend` in the **Repository name** field.
4. Keep the default system settings (including Image tag settings as _Mutable_ and Encryption settings as _AES-256_).
5. Scroll to the bottom of the page and click the orange **Create** button to complete the initialization process.

![Creating ECR Repository](/images/5-Workshop/5.4/5.4.1/Screenshot%202026-09-26%20020124.png)
![Creating ECR Repository](/images/5-Workshop/5.4/5.4.1/Screenshot%202026-09-26%20020155.png)
![Creating ECR Repository](/images/5-Workshop/5.4/5.4.1/Screenshot%202026-09-26%20020718.png)
![Creating ECR Repository](/images/5-Workshop/5.4/5.4.1/Screenshot%202026-09-26%20020727.png)
![Creating ECR Repository](/images/5-Workshop/5.4/5.4.1/Screenshot%202026-09-26%20020735.png)
![Creating ECR Repository](/images/5-Workshop/5.4/5.4.1/Screenshot%202026-09-26%20020801.png)

### Step 2: Retrieve the Deployment Commands (Push Commands)

1. In the Repositories list, select the `eshop-backend` repository you just created.
2. Click **View push commands** in the top-right corner. AWS will provide the standard set of commands to authenticate, build, and push the Image.

![View Push Commands](/images/5-Workshop/5.4/5.4.1/Screenshot%202026-09-26%20021212.png)
![View Push Commands](/images/5-Workshop/5.4/5.4.1/Screenshot%202026-09-26%20021225.png)
![View Push Commands](/images/5-Workshop/5.4/5.4.1/Screenshot%202026-09-26%20021300.png)

### Step 3: Build and Push the Image to ECR

Through Terminal/Command Prompt (run from the project's root directory where the `Dockerfile` is located), the build and push process is executed sequentially through the following commands:

1. **Authenticate the Docker client with Amazon ECR (using AWS CLI):**

```bash
   aws ecr get-login-password --region ap-southeast-1 | docker login --username AWS --password-stdin 753695657650.dkr.ecr.ap-southeast-1.amazonaws.com
```

2. **Build the Docker Image from the source code:**

```bash
   docker build -t eshop-backend .
```

3. **Tag the Image to prepare it for pushing to the repository:**

```bash
   docker tag eshop-backend:latest 753695657650.dkr.ecr.ap-southeast-1.amazonaws.com/eshop-backend:latest
```

4. **Push the Image to Amazon ECR:**
```bash
   docker push 753695657650.dkr.ecr.ap-southeast-1.amazonaws.com/eshop-backend:latest
```
![Building and Pushing the Image to ECR](/images/5-Workshop/5.4/5.4.1/Screenshot%202026-09-26%20024327.png)
![Building and Pushing the Image to ECR](/images/5-Workshop/5.4/5.4.1/Screenshot%202026-09-26%20025524.png)
![Building and Pushing the Image to ECR](/images/5-Workshop/5.4/5.4.1/Screenshot%202026-09-26%20025811.png)

Once the upload process reaches 100%, verify it on the ECR interface. The Image tagged `latest` will appear in the `eshop-backend` repository, confirming that the packaging and storage process was successful.