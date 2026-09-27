---
title: "Monitoring & Auto Scaling"
date: 2026-09-24
weight: 6
chapter: false
pre: " <b> 3.6. </b> "
---

# 3.6. Monitoring & Auto Scaling

In actual operations, completing the infrastructure setup is merely the initial phase. During high-traffic events (such as Flash Sales), the E-shop system must be capable of efficiently handling massive volumes of incoming connections.

This chapter outlines the configuration of **ECS Service Auto Scaling** to enable the system to automatically provision additional Backend Containers (scale out) when CPU load or network traffic increases, and subsequently scale down (scale in) when traffic subsides to optimize operational costs. Furthermore, **Amazon CloudWatch** is implemented to continuously monitor overall system health and trigger automated email alerts (Alarms). Lastly, **AWS CloudTrail** is activated to record detailed audit logs of all infrastructure configuration changes, ensuring strict adherence to security and compliance standards.

---

### Detailed deployment sections:

- **[3.6.1. Configuring CloudWatch Logs & Alarms](3.6.1-cloudwatch-alarms/)**
- **[3.6.2. Configuring ECS Service Auto Scaling by Load (Traffic/CPU)](3.6.2-service-autoscaling/)**
- **[3.6.3. Enabling AWS CloudTrail for API Auditing](3.6.3-cloudtrail-audit/)**