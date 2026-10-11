---
title: "Week 8 Worklog"
date: 2026-09-19
weight: 8
chapter: false
pre: " <b> 1.8. </b> "
---

### Objectives Week 8:

- Deploy the complete E-shop on AWS with an ECS and Serverless architecture.
- Combine VPC, IAM, S3, ECR, EC2 Auto Scaling, ECS, ALB, RDS PostgreSQL, Lambda, CloudWatch, SNS and CloudTrail.
- Troubleshoot integration problems, clean up resources and complete the report and workshop.

### Tasks to be carried out this week:

| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| Sat – Sun | Finalize the architecture (S3 frontend, ECS backend behind ALB in private subnets, RDS PostgreSQL, Lambda image processing, monitoring) and list the services, cost risks and deployment order. | 19/09/2026 | 20/09/2026 | <https://github.com/Vinh-tho/Eshop.git> |
| Mon – Thu | Prepare the source: clone the repository, build the Angular app (eShop.Web), verify the .NET backend with PostgreSQL, review the Dockerfile and plan the deployment steps for the workshop. | 21/09/2026 | 24/09/2026 | <https://github.com/Vinh-tho/Eshop.git> |
| Fri | Evening: build the network foundation (Eshop-VPC, 4 subnets, IGW, NAT Gateway, 2 Route Tables); create IAM Roles for EC2, ECS Task Execution and Lambda; create Eshop-ALB-SG and Eshop-Backend-SG; create the frontend S3 bucket with static hosting and Bucket Policy. | 25/09/2026 | 25/09/2026 | <https://cloudjourney.awsstudygroup.com/> |
| Sat | Early morning: create the media bucket (policy and CORS), upload the Angular build to the frontend bucket and verify the website endpoint; create the ECR repository and build, tag and push the backend image. | 26/09/2026 | 26/09/2026 | <https://cloudjourney.awsstudygroup.com/> |
| Sat | Morning: create the Launch Template and ASG (desired 2), ECS cluster, Capacity Provider, Target Group, ALB, Task Definition and Service (2 tasks); deploy RDS PostgreSQL in a private subnet with a database Security Group allowing port 5432 only from the backend; fix deployment errors (see below) until tasks are Running and Healthy. | 26/09/2026 | 26/09/2026 | <https://cloudjourney.awsstudygroup.com/> |
| Sat | Evening: create the Lambda function and S3 Event Notification and verify logs on CloudWatch; create the SNS topic, CPU alarm and CloudTrail trail; configure ECS Service Auto Scaling (target tracking CPU 70%, 2–10 tasks). | 26/09/2026 | 26/09/2026 | <https://cloudjourney.awsstudygroup.com/> |
| Sun | Clean up in dependency order (ALB/Target Group → ASG/Launch Template → ECS/ECR → Lambda/S3 → NAT Gateway/Elastic IP → VPC), complete the Workshop website, the report and the self-assessment. | 27/09/2026 | 27/09/2026 | <https://cloudjourney.awsstudygroup.com/> |

### Technical configuration details:

| Component | Configuration / value |
| --- | --- |
| Frontend bucket | eshop-frontend-eshop-web; static hosting index.html / error.html; contents of dist/e-shop.web/browser uploaded to the bucket root |
| Media bucket | eshop-media-eshop-web; event notification → Eshop-Image-Resizer |
| Database | RDS PostgreSQL in private subnets, security group SG-DB allowing 5432 only from the backend security group |
| ECS Service Auto Scaling | Min 2, max 10 tasks; target tracking on ECSServiceAverageCPUUtilization = 70; scale-out and scale-in cooldown 60 s; policy name Scale-Out-High-CPU |
| Chain reaction | CPU above 70% → ECS adds a task → no capacity on the EC2 hosts → Capacity Provider signals the ASG → new EC2 joins the cluster → task placed |
| Demo link | http://eshop-frontend-eshop-web.s3-website-ap-southeast-1.amazonaws.com |
| Source code | https://github.com/Vinh-tho/Eshop.git |
| Cleanup order | ALB and Target Group → ASG and Launch Template → ECS service, cluster, task definitions, ECR → Lambda, S3 buckets, CloudWatch log group → NAT Gateway (wait for Deleted) → Elastic IP → VPC |

### Knowledge gained:

- Integration work means debugging across services: read the error message, locate the layer (network, permission, resource size), change one thing at a time and re-test.
- Resource sizing matters: SQL Server was too heavy for a Free Tier instance, which showed up as tasks that never reached Healthy.
- Defence in depth: ALB in public subnets, backend and database in private subnets, Security Group chaining ALB → backend → database, IAM roles instead of keys.
- Cleanup follows dependency order; NAT Gateway must reach Deleted before its Elastic IP can be released, and buckets must be empty before deletion.

### Commands and code used:

**Build the Angular frontend**

```bash
cd eShop.Web
npm install
npm run build   # or: ng build
# output: dist/e-shop.web/browser
```

### Difficulties and solutions:

- "Unable to assume the service linked role" when creating the ECS cluster with an ASG on a new account. → Created the cluster in Fargate-only mode first so AWS generates the service-linked roles, then added the Capacity Provider and ASG.
- ALB could not reach containers because of bridge-mode dynamic ports. → Host port 0, Target Group type Instances, All TCP inbound on Eshop-Backend-SG from Eshop-ALB-SG.
- ECS Task never became Running/Healthy with SQL Server (too heavy for Free Tier instances). → Switched to the lighter PostgreSQL, adjusted the connection settings; the Service then passed the Target Group health checks.
- S3 website returned 403 or showed nothing. → Disabled Block Public Access, added a GetObject policy ending in /*, and uploaded the contents of dist/e-shop.web/browser so index.html sits at the bucket root.
- Elastic IP could not be released (in use) and a bucket could not be deleted. → Delete the NAT Gateway first and wait for Deleted (3–5 minutes) before releasing the EIP; empty buckets (including resized/) before deleting them.

### Achievements Week 8:

- Delivered the complete E-shop: static frontend on S3, containerized .NET backend on ECS (EC2 Launch Type) behind an ALB in private subnets, RDS PostgreSQL, event-driven Lambda, monitoring and auditing.
- Applied least-privilege IAM roles and Security Group chaining (ALB SG → Backend SG → DB SG).
- Published a Workshop guide of 7 chapters (3.1–3.7), the demo link, the source code and the report.
- Cleaned up all billed resources to avoid unexpected charges.

### Deliverables and evidence:

- Working demo link (before cleanup) and screenshots for every chapter 3.1–3.7.
- Workshop website with 7 chapters, GitHub source code and the final report.
- Self-assessment and student feedback form completed.

### Plan for next week:

- Continue with CI/CD and Infrastructure as Code, and prepare for AWS Certified Cloud Practitioner.
