---
title: "Proposal"
date: 2026-09-24
weight: 2
chapter: false
pre: "<b> 2. </b>"
---

# Scalable and Optimized Web E-shop System on AWS

## Container Architecture on EC2 & Event-Driven Design for an E-commerce Platform

### 1. Executive Summary

This proposal presents the overall architectural solution for deploying the **Web E-shop** project on AWS cloud infrastructure.

**Core Architecture:** The system is designed using a hybrid model combining **Static Hosting**, **EC2-based Containerization**, and **Serverless** computing: the Frontend and static assets are hosted on **Amazon S3**; the Backend is packaged with **Docker** and deployed on **Amazon ECS running on EC2 instances** (EC2 Auto Scaling Group combined with an ECS Capacity Provider); background processing tasks are handled by **AWS Lambda**. The entire system resides within a **VPC**, clearly separated into a **Public Subnet** (ALB) and a **Private Subnet** (EC2/ECS Backend), and is monitored by **CloudWatch & CloudTrail**.

Rather than running the entire application on a single traditional server (monolithic architecture), the project separates the Frontend and Backend, containerizes the Backend, and automates auxiliary tasks (product image processing) using Lambda to reduce the load on the main server.

---

### 2. Problem Statement

#### Challenges of Traditional E-shop Systems

- **Inability to Handle Traffic Spikes:** During flash sales or major promotions, sudden surges in traffic can easily crash web servers with fixed configurations, leading to lost revenue and a poor customer experience.
- **Suboptimal Maintenance Costs:** Servers must be provisioned with excess capacity to prepare for peak periods, wasting resources during normal days or at night when traffic is low.
- **Poor Static Content Performance:** Forcing the Backend server to process and return both interface files (HTML/CSS/JS) and product images reduces the speed at which it can handle core transactions (such as cart operations and checkout).
- **Difficulty with Deployment Updates:** Deploying new features on a fixed-server architecture is often complex and carries the risk of service disruption.

#### Proposed Solution

The system is restructured into independent components (Decoupled Architecture), applying AWS's core services:

1. **Frontend on Amazon S3:** The user interface (Web Client) and product images are stored as static assets on **Amazon S3**, enabling fast page loads, scalability, and low cost.
2. **EC2-based Backend Containerization:** The Backend API (handling cart logic, checkout, and product management) is packaged with **Docker** and runs on **Amazon ECS with the EC2 launch type**. The EC2 instances underlying ECS are managed by an **EC2 Auto Scaling Group**, combined with an **ECS Capacity Provider** to ensure sufficient resources for running Tasks. In front of this sits an **Application Load Balancer (ALB)**, which distributes requests to Tasks via a Target Group.
3. **Background Processing with Lambda:** When an Admin uploads a product image to S3, this event triggers **AWS Lambda** to automatically process (resize) the image — a典型 example of an Event-driven Serverless architecture.
4. **Security & Monitoring:** The entire Backend (EC2 + ECS) resides within a **Private Subnet** of the **VPC**, with no direct exposure to the Internet — it only receives traffic that has passed through the ALB in the Public Subnet. All activity is logged via **CloudTrail** and monitored via **CloudWatch**.

#### Expected Benefits

- **Flexible scalability as traffic increases:** ECS Service Auto Scaling adjusts the number of Tasks; when EC2 capacity is insufficient, the ECS Capacity Provider works with the EC2 Auto Scaling Group to add EC2 instances.
- **Potential for cost optimization:** S3 and Lambda apply pay-as-you-go pricing models based on actual usage; the EC2 Auto Scaling Group allows the number of instances to be adjusted according to demand, rather than maintaining a large, fixed number of servers.
- **Customer data security:** Isolating the data-processing server from the public Internet and restricting access through Security Groups and IAM.

---

### 3. Solution Architecture

#### 3.1. Current State vs. Target Architecture

Since the current system is **not yet connected to AWS**, this section clearly distinguishes between the current state and the proposed target.

**Current State:**

```text
Web E-shop
    │
    ▼
 Backend
    │
    ▼
 Database
```

**Target Architecture (proposed for AWS deployment):**

```text
                         INTERNET
                             │
              ┌──────────────┴──────────────┐
              │                             │
              ▼                             ▼
        Amazon S3                    Application Load
   Frontend + Media                     Balancer
                                            │
                                            ▼
                                  ┌──────────────────┐
                                  │   PUBLIC SUBNET  │
                                  │       ALB        │
                                  └────────┬─────────┘
                                           │
                                           ▼
                                  ┌──────────────────┐
                                  │  PRIVATE SUBNET  │
                                  │                  │
                                  │   ECS Cluster    │
                                  │       │          │
                                  │   EC2 Instances  │
                                  │       │          │
                                  │  Docker Backend  │
                                  │                  │
                                  │    Database      │
                                  └──────────────────┘
                                           ▲
                                           │
                              EC2 Auto Scaling Group
                                           ▲
                                           │
                                  ECS Capacity Provider

        Amazon S3 (Media)
              │
              │ Event Notification
              ▼
         AWS Lambda
              │
              ▼
       Image Processing
              │
              ▼
        Amazon S3 (Media)


     CloudWatch ─────── Monitoring / Logs / Alarms
     CloudTrail ─────── AWS API Audit
     IAM ────────────── Access Control
```

#### 3.2. Overall Architecture Diagram

![E-shop Architecture Diagram](/images/2-Proposal/eshop_architecture.png)

#### 3.3. Breakdown of Core Processing Flows (4 Flows)

##### Flow F — Frontend & Customer Experience

```text
F1: User accesses the E-shop
        ↓
F2: Amazon S3 serves static Frontend assets
    (HTML / CSS / JavaScript / images)
        ↓
F3: Browser calls the Backend API via the Application Load Balancer
        ↓
F4: Backend processes the business logic and returns data to the Client
```

##### Flow B — Backend API & Transaction Processing (Core Business)

```text
B1: Client sends an API Request
        ↓
B2: Application Load Balancer (ALB) receives the request
        ↓
B3: ALB routes the Request to the ECS Service via a Target Group
        ↓
B4: ECS Service forwards the Request to the Docker Container running on EC2
        ↓
B5: Backend processes the business logic
        ↓
B6: Reads/writes to the Database or S3
```

**Accompanying scaling mechanism (Auto Scaling):**

```text
Traffic increases
    ↓
ECS Service Auto Scaling
    ↓
Increases the number of ECS Tasks
    ↓
If EC2 capacity is insufficient
    ↓
ECS Capacity Provider
    ↓
EC2 Auto Scaling Group
    ↓
Launches additional EC2 instances
```

> **Note:** The ALB is responsible for distributing requests; ECS Service Auto Scaling adjusts the number of Tasks/Containers; the EC2 Auto Scaling Group only adjusts the number of EC2 instances underlying ECS.

##### Flow E — Event-Driven Background Processing (Serverless)

```text
E1: Admin uploads a product image to S3
        ↓
E2: S3 Event Notification
        ↓
E3: AWS Lambda is triggered
        ↓
E4: Resizes the image (Thumbnail / Medium / Large)
        ↓
E5: Saves the results back to S3
```

##### Flow S — Security & Monitoring

```text
S1: EC2 / ECS / Lambda generate logs & metrics
        ↓
S2: CloudWatch collects Logs and Metrics
        ↓
S3: Dashboard / Alarm (email alert when HTTP 500 errors spike)

AWS API Calls
      ↓
  CloudTrail
      ↓
  Audit Log
```

- **IAM:** Applies the principle of least privilege — for example, the ECS Task Role is only permitted to read/write to the S3 Bucket dedicated to product images, and the EC2 instance role is not granted unnecessary administrative permissions.

---

### 4. Technical Implementation

#### Implementation Phases

1. **Phase 1: Network Foundation (Networking) & Security**
   - Create a dedicated **VPC** for the project.
   - Divide the network into Public Subnets (for the ALB) and Private Subnets (for the EC2/ECS Backend and Database).
   - Configure the **Internet Gateway (IGW)**, NAT Gateway, and routing (Route Tables).
   - Create **IAM Roles** and Security Groups following the principle of least privilege.

2. **Phase 2: Frontend and Digital Asset Storage**
   - Configure **Amazon S3** to host the static website.
   - Create a second S3 Bucket to store Media (product images and videos).

3. **Phase 3: Containerization and EC2-based Backend Deployment**
   - Write a `Dockerfile` to package the Backend source code.
   - Create a **Launch Template** and an **EC2 Auto Scaling Group** to serve as the container runtime foundation.
   - Create an **ECS Cluster (EC2 launch type)**, Task Definitions, and configure an **ECS Capacity Provider** linked to the Auto Scaling Group.
   - Configure the **Application Load Balancer (ALB)** and Target Group connected to the ECS Service.
   - Set up **ECS Service Auto Scaling** based on CPU/traffic metrics.

4. **Phase 4: Serverless Integration and Automation**
   - Write **AWS Lambda** code (Node.js/Python) to process product images.
   - Configure S3 Event Notifications to automatically trigger Lambda when a new file is uploaded.

5. **Phase 5: System Monitoring**
   - Stream logs from EC2/ECS to **CloudWatch Logs**.
   - Set up CloudWatch Alarms to alert on errors or when resources (CPU) are nearing exhaustion.
   - Enable **CloudTrail** to audit infrastructure activity.

---

### 5. Timeline & Milestones

```text
+-----------------------------------------------------------------------------------+
| Week 1: Network Architecture & Static Storage Setup                               |
|   - Create VPC, Public/Private Subnets, Internet Gateway, Security Groups.        |
|   - Set up S3 Hosting for the Frontend and S3 for Media Storage.                  |
|   - Create the necessary IAM Policies.                                            |
+-----------------------------------------------------------------------------------+
                                      |
                                      v
+-----------------------------------------------------------------------------------+
| Week 2-3: EC2, Docker, ECS & Load Balancing (Core Backend)                        |
|   - Create the Launch Template & EC2 Auto Scaling Group.                          |
|   - Build the Docker Image for the Backend.                                       |
|   - Set up the ECS Cluster (EC2 launch type) & ECS Capacity Provider.             |
|   - Configure the Application Load Balancer (ALB) and Target Group.               |
|   - Set up ECS Service Auto Scaling and the EC2 Auto Scaling Group.               |
+-----------------------------------------------------------------------------------+
                                      |
                                      v
+-----------------------------------------------------------------------------------+
| Week 4: Serverless Integration, Finalization & Monitoring                         |
|   - Write and deploy the AWS Lambda function for product image processing.        |
|   - Connect the Event Trigger from S3 to Lambda.                                  |
|   - Build the CloudWatch Dashboard and configure Alarms.                          |
|   - Test the entire system (load testing simulating a Flash Sale).                |
+-----------------------------------------------------------------------------------+
```

---

### 6. Budget Estimate (Standard Architecture)

The combined **EC2/ECS and Serverless** architecture provides high stability with flexible costs:

| AWS Service                   | Purpose / Estimated Scale                               | Cost Allocation / Nature                                                                     |
| ----------------------------- | ------------------------------------------------------- | -------------------------------------------------------------------------------------------- |
| **Amazon S3**                 | Static Frontend hosting and product image storage       | Very low (Pay per GB)                                                                        |
| **Amazon VPC**                | VPC, Subnet, IGW, Security Groups, NAT Gateway          | Basic VPC is free; the NAT Gateway and certain related networking components may incur costs |
| **Application Load Balancer** | Distributes incoming traffic to the Backend             | Fixed hourly cost + data processed                                                           |
| **Amazon EC2 + Amazon ECS**   | EC2 instances serving as the base for Docker containers | Cost based on instance type and running hours                                                |
| **AWS Lambda**                | Resizes product images upon upload events               | Pay per invocation                                                                           |
| **CloudWatch / CloudTrail**   | Log storage and alerting                                | Primarily based on log storage volume                                                        |

> **Note:** Cost estimates are for reference only and depend on the Region, resource configuration (EC2 instance type and count), running time, and actual usage volume.

---

### 7. Risk Assessment

| Potential Risk                                        | Level  | Mitigation Strategy                                                                                                                                                                       |
| ----------------------------------------------------- | ------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Massive customer traffic surge (Flash Sale)**       | High   | The ALB, combined with ECS Service Auto Scaling and the EC2 Auto Scaling Group, can automatically adjust the number of Tasks/EC2 instances based on load metrics.                         |
| **Container / Source Code Crashes**                   | Medium | ECS can detect unhealthy tasks/containers via Health Checks and launch replacement tasks, maintaining the system's ability to serve requests.                                             |
| **Risk of direct attacks on the DB/Backend**          | High   | The Backend (EC2/ECS) runs in a Private Subnet and only receives traffic that has passed through the ALB in the Public Subnet. The Database is also not exposed directly to the Internet. |
| **Log loss or errors**                                | Medium | CloudWatch Logs is used to centralize logs, and CloudTrail tracks AWS API activity.                                                                                                       |
| **Sudden cost spikes (due to extensive EC2 scaling)** | Medium | Set up CloudWatch monitoring, limit the maximum number of instances in the Auto Scaling Group, and configure cost alerts (AWS Budgets).                                                   |

---

### 8. Expected Outcomes

1. **Successful deployment of the Web E-shop on AWS:** Migrate the current Web E-shop system to the AWS environment and connect the Frontend, Backend, and storage components according to the proposed architecture.
2. **Application of AWS knowledge:** Apply knowledge of **EC2, S3, IAM, VPC, Lambda, CloudWatch, CloudTrail, ELB, Auto Scaling, ECS, and Docker** to a real-world system.
3. **Scalability:** The Backend can scale the number of ECS Tasks as traffic increases via ECS Service Auto Scaling; when EC2 capacity is insufficient, the ECS Capacity Provider works with the EC2 Auto Scaling Group to add EC2 instances.
4. **Automation:** AWS Lambda handles background tasks (product image processing) triggered by S3 events, reducing the load on the main Backend.
5. **Foundation for future expansion:** The architecture allows for the future addition of components such as Amazon CloudFront, Amazon RDS/Aurora, Amazon SES, Amazon SQS, or a CI/CD pipeline without requiring changes to the entire system.
