---
title: "Week 3 Worklog"
date: 2026-08-15
weight: 3
chapter: false
pre: " <b> 1.3. </b> "
---

### Objectives Week 3:

- Learn AWS Networking: VPC, Subnet, Internet Gateway, NAT Gateway and Route Table.
- Design and build a VPC with public and private subnets across two Availability Zones.
- Control traffic with Security Groups and understand how they differ from Network ACLs.

### Tasks to be carried out this week:

| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| Sat – Sun | Study CIDR notation, subnetting, public vs private subnet, Internet Gateway, NAT Gateway and Route Table. | 15/08/2026 | 16/08/2026 | <https://docs.aws.amazon.com/vpc/latest/userguide/> |
| Mon – Tue | Create VPC 10.0.0.0/16 and four /24 subnets (2 public, 2 private) in ap-southeast-1a and ap-southeast-1b; enable auto-assign public IPv4 on public subnets. | 17/08/2026 | 18/08/2026 | <https://cloudjourney.awsstudygroup.com/> |
| Wed | Create and attach an Internet Gateway; create a NAT Gateway in a public subnet with an Elastic IP. | 19/08/2026 | 19/08/2026 | <https://cloudjourney.awsstudygroup.com/> |
| Thu | Create two Route Tables: public (0.0.0.0/0 → IGW) and private (0.0.0.0/0 → NAT); associate them with the correct subnets. Study Security Group (stateful) vs Network ACL (stateless). | 20/08/2026 | 20/08/2026 | <https://cloudjourney.awsstudygroup.com/> |
| Fri | Launch EC2 instances in the VPC and verify Internet access paths; note that NAT Gateway and Elastic IP are billed hourly and must be cleaned up after practice. | 21/08/2026 | 21/08/2026 | <https://cloudjourney.awsstudygroup.com/> |

### Technical configuration details:

| Component | Configuration / value |
| --- | --- |
| VPC | Eshop-VPC, IPv4 CIDR 10.0.0.0/16 |
| Eshop-Public-Subnet-1 | ap-southeast-1a, 10.0.1.0/24, auto-assign public IPv4 enabled |
| Eshop-Public-Subnet-2 | ap-southeast-1b, 10.0.2.0/24, auto-assign public IPv4 enabled |
| Eshop-Private-Subnet-1 | ap-southeast-1a, 10.0.3.0/24 |
| Eshop-Private-Subnet-2 | ap-southeast-1b, 10.0.4.0/24 |
| Internet Gateway | Eshop-IGW, attached to Eshop-VPC |
| NAT Gateway | Eshop-NAT-GW in Eshop-Public-Subnet-1, connectivity type Public, new Elastic IP |
| Eshop-Public-RT | 0.0.0.0/0 → Eshop-IGW; associated with both public subnets |
| Eshop-Private-RT | 0.0.0.0/0 → Eshop-NAT-GW; associated with both private subnets |
| Eshop-ALB-SG | Inbound HTTP 80 and HTTPS 443 from 0.0.0.0/0; outbound all traffic |
| Eshop-Backend-SG | Inbound All TCP only from Eshop-ALB-SG (supports ECS dynamic port mapping) |

### Knowledge gained:

- A /16 network provides 65,536 addresses and a /24 subnet 256 (AWS reserves 5 per subnet), so 10.0.0.0/16 split into four /24 blocks leaves plenty of room to grow.
- A subnet is public only because its route table sends 0.0.0.0/0 to an Internet Gateway; a private subnet sends it to a NAT Gateway instead.
- The NAT Gateway must live in a public subnet and needs an Elastic IP; it allows outbound-only connections from private resources.
- Security Group is stateful and attached to resources; Network ACL is stateless and attached to subnets. The project uses Security Groups only.
- Referencing a Security Group as a source (Backend SG ← ALB SG) is safer than opening IP ranges: only traffic coming through the load balancer is accepted.
- Two AZs for every tier give high availability; NAT Gateway and Elastic IP are billed hourly.

### Difficulties and solutions:

- Route tables and subnet associations were abstract and easy to confuse. → Drew the traffic path (user → IGW → ALB; container → NAT → Internet) before configuring.

### Achievements Week 3:

- Built a VPC with 2 public and 2 private subnets, an Internet Gateway, a NAT Gateway and two Route Tables.
- Understood the one-way outbound role of NAT Gateway versus the two-way role of Internet Gateway.
- Understood the difference between Security Group and Network ACL.
- Identified hourly-billed networking components and the need for a cleanup routine.

### Deliverables and evidence:

- VPC resource map showing 4 subnets, IGW, NAT Gateway and 2 route tables.
- Route table entries and subnet associations verified on the Console.
- Two Security Groups ready for the ALB and the backend.

### Plan for next week:

- Learn AWS Lambda and build an image-processing function triggered by S3.
