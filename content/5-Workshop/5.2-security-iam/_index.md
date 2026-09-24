---
title: "Basic Security Setup (IAM & SG)"
date: 2026-09-24
weight: 2
chapter: false
pre: " <b> 5.2. </b> "
---

# 5.2. Setting Up Basic Security (IAM & Security Groups)

In this chapter, we will configure the foundational security layers for the Web E-shop system following the AWS principle of least privilege. Specifically, we will create **IAM Roles** for EC2, ECS Tasks, and Lambda to grant them the necessary permissions to interact with other AWS services (such as S3, ECR, and CloudWatch). Additionally, we will set up **Security Groups** to act as virtual firewalls, strictly controlling the network traffic flow between the Load Balancer in the Public Subnet and the Backend Containers in the Private Subnet.

---

### List of detailed practical exercises:

- **[5.2.1. Assigning IAM Roles for EC2, ECS Tasks, and Lambda](5.2.1-iam-roles/)**
- **[5.2.2. Creating Security Groups for ALB and ECS Backend](5.2.2-security-groups/)**