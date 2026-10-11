---
title: "Week 1 Worklog"
date: 2026-08-01
weight: 1
chapter: false
pre: " <b> 1.1. </b> "
---

### Objectives Week 1:

- Understand the First Cloud AI Journey (FCAJ) program, its 8-week roadmap and working rules.
- Learn Cloud Computing fundamentals and the global infrastructure of AWS (Region, Availability Zone, Edge).
- Create an AWS Free Tier practice account and become familiar with the AWS Management Console.
- Sketch the first high-level architecture for the E-shop project (Static Hosting, Container Backend, Event-Driven Serverless).

### Tasks to be carried out this week:

| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| Sat – Sun | Join the program orientation: company introduction, 8-week roadmap, evaluation rules (CLO assessment every 4 weeks), contact the mentor. | 01/08/2026 | 02/08/2026 | <https://cloudjourney.awsstudygroup.com/> |
| Sat – Sun | Study Cloud Computing concepts: IaaS/PaaS/SaaS, pay-as-you-go model, shared responsibility model. | 01/08/2026 | 02/08/2026 | <https://cloudjourney.awsstudygroup.com/> |
| Mon | Create and verify the AWS Free Tier account, learn how Free Tier limits and Billing work to avoid unexpected charges. | 03/08/2026 | 03/08/2026 | <https://cloudjourney.awsstudygroup.com/> |
| Tue – Wed | Explore the Management Console; learn the main service groups (Compute, Storage, Networking, Database, Security) and how to switch Region (ap-southeast-1, Singapore). | 04/08/2026 | 05/08/2026 | <https://cloudjourney.awsstudygroup.com/> |
| Thu | Study the global infrastructure (Region, AZ, Edge Location) and why high availability requires at least two AZs. | 06/08/2026 | 06/08/2026 | <https://cloudjourney.awsstudygroup.com/> |
| Fri | Read the E-shop source structure on GitHub (Angular frontend, .NET backend, PostgreSQL) and draw the first architecture sketch with three parts: static hosting, container backend, event-driven image processing. | 07/08/2026 | 07/08/2026 | <https://github.com/Vinh-tho/Eshop.git> |

### Technical configuration details:

| Component | Configuration / value |
| --- | --- |
| Region | ap-southeast-1 (Asia Pacific – Singapore), closest to users in Vietnam |
| Account type | AWS Free Tier practice account |
| Frontend (planned) | Angular build → Amazon S3 Static Website Hosting |
| Backend (planned) | Dockerized .NET → Amazon ECS (EC2 Launch Type) behind an Application Load Balancer, in private subnets |
| Database (planned) | PostgreSQL |
| Image processing (planned) | S3 Event → AWS Lambda (event-driven) |
| Operations (planned) | CloudWatch + SNS (alerts), CloudTrail (audit), IAM roles (least privilege) |

### Knowledge gained:

- Cloud service models: IaaS (e.g. EC2), PaaS and SaaS; in the E-shop, S3 is storage, ECS/EC2 is container compute and Lambda is function-as-a-service.
- Shared responsibility model: AWS secures the cloud itself (data centers, hardware); the customer secures what runs in it (IAM, Security Groups, data, application).
- Region vs Availability Zone: a Region is a geographic area, an AZ is an isolated data center inside it; using two AZs lets the system survive the loss of one.
- Pay-as-you-go pricing: some services are billed by time even when idle (NAT Gateway, Elastic IP, Application Load Balancer), so every lab must end with cleanup.

### Difficulties and solutions:

- Many new terms appear at once (Region, AZ, IaaS, shared responsibility). → Kept a personal glossary and re-explained each term in my own words.

### Achievements Week 1:

- Understood the main components of the AWS global infrastructure and the Console workflow.
- Created and verified an AWS Free Tier practice account.
- Identified which AWS services are inside and outside the Free Tier, and prepared a cost-awareness habit for later weeks.
- Produced a first architecture draft for the E-shop project.

### Deliverables and evidence:

- Personal glossary of cloud terms.
- First architecture sketch of the E-shop (three parts: static hosting, container backend, event-driven serverless).
- Verified AWS account ready for hands-on labs.

### Plan for next week:

- Study EC2, S3 and IAM and practice with real resources.
