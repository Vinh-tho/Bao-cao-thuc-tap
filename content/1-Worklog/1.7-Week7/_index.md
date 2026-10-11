---
title: "Week 7 Worklog"
date: 2026-09-12
weight: 7
chapter: false
pre: " <b> 1.7. </b> "
---

### Objectives Week 7:

- Learn Docker basics and container orchestration with Amazon ECS.
- Write a Dockerfile for the .NET backend, build the image and push it to Amazon ECR.
- Create an ECS cluster (EC2 Launch Type), Capacity Provider, Task Definition and Service connected to the ALB.

### Tasks to be carried out this week:

| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| Sat – Sun | Study Docker: image, container, Dockerfile, registry; practice docker build/run/tag/push. | 12/09/2026 | 13/09/2026 | <https://docs.docker.com> |
| Mon – Tue | Study container orchestration and ECS concepts: cluster, task definition, service, EC2 vs Fargate launch types, Capacity Provider, bridge network mode and dynamic port mapping. | 14/09/2026 | 15/09/2026 | <https://docs.aws.amazon.com/AmazonECS/latest/developerguide/> |
| Wed – Thu | Write the Dockerfile for the .NET backend, build the image locally, test it, create the ECR repository eshop-backend, authenticate with AWS CLI and push the image. | 16/09/2026 | 17/09/2026 | <https://cloudjourney.awsstudygroup.com/> |
| Fri | Create the ECS cluster, Capacity Provider linked to the ASG, Task Definition (bridge mode, host port 0) and Service with two tasks behind the ALB; verify tasks reach Running. | 18/09/2026 | 18/09/2026 | <https://cloudjourney.awsstudygroup.com/> |

### Technical configuration details:

| Component | Configuration / value |
| --- | --- |
| ECR repository | eshop-backend, private, tag mutability Mutable, encryption AES-256 |
| ECS cluster | Eshop-ECS-Cluster (created Fargate-only first so service-linked roles exist) |
| Capacity Provider | Eshop-ECS-CP, scaling type EC2 Auto Scaling, group Eshop-ECS-ASG; state Active |
| Eshop-Backend-Task | Launch type EC2, network mode bridge, 0.5 vCPU, 0.5 GB, task role and execution role Eshop-ECS-Task-Execution-Role |
| Container | eshop-backend-container, image = ECR URI :latest, container port 80, host port 0 (dynamic mapping), protocol TCP |
| Eshop-Backend-Service | Capacity provider strategy Eshop-ECS-CP, desired tasks 2, load balancer Eshop-ALB, listener HTTP:80, target group Eshop-Backend-TG |

### Knowledge gained:

- Image vs container: an image is the packaged application; a container is a running instance of it.
- Task Definition is the blueprint (image, CPU/RAM, ports, roles); a Service keeps the desired number of tasks running and registers them with the load balancer.
- Bridge mode with host port 0 lets several containers share one EC2 host without port conflicts, which is why the backend SG allows All TCP from the ALB SG.
- A Capacity Provider links the ECS cluster to the ASG so ECS can ask for more EC2 instances when tasks do not fit.
- A t2.micro host has limited memory, so tasks were sized at 0.5 vCPU / 0.5 GB.

### Commands and code used:

**Authenticate, build, tag and push the backend image**

```bash
aws ecr get-login-password --region ap-southeast-1 | docker login --username AWS --password-stdin <ACCOUNT_ID>.dkr.ecr.ap-southeast-1.amazonaws.com
docker build -t eshop-backend .
docker tag eshop-backend:latest <ACCOUNT_ID>.dkr.ecr.ap-southeast-1.amazonaws.com/eshop-backend:latest
docker push <ACCOUNT_ID>.dkr.ecr.ap-southeast-1.amazonaws.com/eshop-backend:latest
```

### Difficulties and solutions:

- Limited Docker background made ECS concepts hard to follow at first. → Practiced Docker locally before touching ECS.

### Achievements Week 7:

- Understood how Docker packages an application and how ECS keeps the desired number of containers running.
- Built and pushed a backend image to ECR.
- Understood why bridge mode needs dynamic port mapping and how the Security Group must allow the ALB.
- Prepared the container pipeline for the final E-shop deployment.

### Deliverables and evidence:

- Image tagged latest visible in the ECR repository.
- Cluster showing two active container instances and tasks in Running state.

### Plan for next week:

- Combine all knowledge into the final E-shop project.
