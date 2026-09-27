---
title: "Building the Backend Container"
date: 2026-09-24
weight: 4
chapter: false
pre: " <b> 3.4. </b> "
---

# 3.4. Building the Backend Container with Docker, Amazon EC2 & ECS

Following the successful deployment of the Frontend on S3, the system requires a robust Backend architecture to process core business logic such as shopping cart management, payment processing, promotion calculation, and product data retrieval.

Instead of executing source code directly on traditional virtual servers (EC2), this architecture will **Containerize** the Backend using Docker and deploy it onto **Amazon ECS (Elastic Container Service)**. This approach enables the E-shop system to easily deploy new software versions without the risk of environmental conflicts (missing libraries, incorrect runtime versions). Additionally, it provides highly flexible Auto Scaling capabilities to effectively handle sudden spikes in network traffic.

---

### Detailed deployment sections:

- **[3.4.1. Packaging Backend (Dockerfile) & Pushing Image to Amazon ECR](3.4.1-docker-ecr/)**
- **[3.4.2. Creating EC2 Launch Template & EC2 Auto Scaling Group](3.4.2-ec2-asg/)**
- **[3.4.3. Initializing ECS Cluster (EC2 Launch Type) & ECS Capacity Provider](3.4.3-ecs-cluster-cp/)**
- **[3.4.4. Configuring Application Load Balancer (ALB) & Target Group](3.4.4-alb-config/)**
- **[3.4.5. Defining ECS Task, Creating Service & Connecting ALB](3.4.5-ecs-service/)**