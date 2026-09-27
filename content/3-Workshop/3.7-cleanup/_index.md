---
title: "Resource Cleanup"
date: 2026-09-24
weight: 7
chapter: false
pre: " <b> 3.7. </b> "
---

# 3.7. Resource Cleanup

The deployment of the E-shop Web project on AWS infrastructure is now complete. To prevent the accrual of unexpected charges (particularly for services billed on an hourly basis, such as NAT Gateways or Application Load Balancers), the cleanup and decommissioning of system resources is a mandatory procedure.

The removal of resources must be executed sequentially in the exact order specified below to avoid dependency errors between associated services.

---

### Detailed cleanup sections:

- **[3.7.1. Cleaning Up Application Load Balancer & Auto Scaling Group](3.7.1-alb-asg-cleanup/)**
- **[3.7.2. Cleaning Up ECS Cluster, Task Definitions & Amazon ECR](3.7.2-ecs-ecr-cleanup/)**
- **[3.7.3. Cleaning Up AWS Lambda & Amazon S3 Buckets](3.7.3-lambda-s3-cleanup/)**
- **[3.7.4. Cleaning Up VPC, NAT Gateway & Elastic IP (Preventing Unexpected Costs)](3.7.4-vpc-nat-cleanup/)**