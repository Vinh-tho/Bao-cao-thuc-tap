---
title: "Building Backend Container"
date: 2026-09-24
weight: 4
chapter: false
pre: " <b> 5.4. </b> "
---

# 5.4. Building the Backend Container with Docker, Amazon EC2 & ECS

Now that the Frontend is live on S3, we need a robust Backend to handle business logic such as adding items to the cart, processing payments, calculating discounts, and retrieving product data.

Instead of running the source code directly on a traditional virtual server (EC2), we will **Containerize** the Backend using Docker and deploy it to **Amazon ECS (Elastic Container Service)**. This approach allows the E-shop to easily update to new versions without worrying about environment discrepancies (missing libraries, wrong Node/Python versions) and provides extremely flexible Auto Scaling capabilities during traffic spikes.

---

### List of detailed practical exercises:

- **[5.4.1. Packaging the Backend (Dockerfile) & Pushing the Image to Amazon ECR](5.4.1-docker-ecr/)**
- **[5.4.2. Creating an EC2 Launch Template & EC2 Auto Scaling Group](5.4.2-ec2-asg/)**
- **[5.4.3. Initializing an ECS Cluster (EC2 Launch Type) & ECS Capacity Provider](5.4.3-ecs-cluster-cp/)**
- **[5.4.4. Configuring the Application Load Balancer (ALB) & Target Group](5.4.4-alb-config/)**
- **[5.4.5. Defining the ECS Task, Creating a Service, & Connecting the ALB](5.4.5-ecs-service/)**