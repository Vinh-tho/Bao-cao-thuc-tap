---
title: "Week 6 Worklog"
date: 2026-09-05
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
---

### Objectives Week 6:

- Learn Elastic Load Balancing (Application Load Balancer) and EC2 Auto Scaling.
- Create a Target Group, an ALB, a Launch Template and an Auto Scaling Group.
- Verify load distribution and automatic scaling behaviour.

### Tasks to be carried out this week:

| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| Sat – Sun | Study ALB vs NLB, listeners, Target Groups, health checks, and Auto Scaling concepts (Launch Template, ASG, desired/min/max, scaling policies). | 05/09/2026 | 06/09/2026 | <https://docs.aws.amazon.com/elasticloadbalancing/latest/application/> |
| Mon – Tue | Create a Target Group (HTTP:80, health check on /) and an Internet-facing ALB across two public subnets with a dedicated ALB Security Group allowing HTTP/HTTPS. | 07/09/2026 | 08/09/2026 | <https://cloudjourney.awsstudygroup.com/> |
| Wed – Thu | Create an EC2 Launch Template (t2.micro, Security Group, IAM instance profile, User data) and an Auto Scaling Group in the private subnets (desired 2, min 1, max 4). | 09/09/2026 | 10/09/2026 | <https://docs.aws.amazon.com/autoscaling/ec2/userguide/> |
| Fri | Test the DNS name of the ALB, check target health, and observe the ASG replacing/adding instances; clean up billed resources afterwards. | 11/09/2026 | 11/09/2026 | <https://cloudjourney.awsstudygroup.com/> |

### Technical configuration details:

| Component | Configuration / value |
| --- | --- |
| Eshop-Backend-TG | Target type Instances, HTTP:80, health check HTTP path /, VPC Eshop-VPC; targets registered later by ECS |
| Eshop-ALB | Internet-facing, IPv4, two public subnets in two AZs, security group Eshop-ALB-SG, listener HTTP:80 → forward to Eshop-Backend-TG |
| Eshop-ECS-Launch-Template | Amazon ECS-optimized Amazon Linux AMI, t2.micro, no key pair, security group Eshop-Backend-SG, IAM instance profile Eshop-EC2-Instance-Role, user data registering the instance to the ECS cluster |
| Eshop-ECS-ASG | Launch template above, VPC Eshop-VPC, private subnets 1 and 2, desired 2 / min 1 / max 4, no load balancer attached (ALB is integrated through ECS) |

### Knowledge gained:

- Target type Instances is used because ECS bridge mode registers the EC2 instance together with a random host port.
- Health checks decide whether the ALB sends traffic to a target; an unhealthy target is taken out of rotation.
- A Launch Template stores the instance blueprint; an Auto Scaling Group keeps the desired number of instances across AZs and replaces failed ones.
- The ECS-optimized AMI already contains Docker and the ECS agent; user data tells the agent which cluster to join.

### Commands and code used:

**User data in the Launch Template**

```bash
#!/bin/bash
echo ECS_CLUSTER=Eshop-ECS-Cluster >> /etc/ecs/ecs.config
```

### Difficulties and solutions:

- Targets stayed unhealthy when Security Group rules did not allow the ALB. → Allowed traffic from the ALB Security Group to the backend Security Group only.

### Achievements Week 6:

- Understood how ALB, Target Group and health checks work together.
- Built an ASG from a Launch Template running in private subnets.
- Verified traffic distribution and instance self-healing/scaling.
- These components were reused for the ECS cluster in week 7 and the final project.

### Deliverables and evidence:

- Target Group and ALB in Active state.
- ASG with two instances running in private subnets.

### Plan for next week:

- Learn Docker and deploy containers on ECS.
