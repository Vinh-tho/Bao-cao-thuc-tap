---
title: "Basic Security Setup (IAM & SG)"
date: 2026-09-24
weight: 2
chapter: false
pre: " <b> 3.2. </b> "
---

# 3.2. Basic Security Setup (IAM & Security Groups)

This chapter outlines the process of configuring the foundational security layers for the E-shop Web system, strictly adhering to the AWS principle of Least Privilege. Specifically, **IAM Roles** will be provisioned for EC2, ECS Tasks, and Lambda to grant them the necessary permissions to interact with other AWS services (such as S3, ECR, and CloudWatch). Concurrently, **Security Groups** will be established to act as virtual firewalls, strictly controlling the flow of network traffic between the Load Balancer in the Public Subnets and the Backend Containers in the Private Subnets.

---

### Detailed Lab List:

- **[3.2.1. Configuring IAM Roles for EC2, ECS Task, and Lambda](3.2.1-iam-roles/)**
- **[3.2.2. Creating Security Groups for ALB and ECS Backend](3.2.2-security-groups/)**