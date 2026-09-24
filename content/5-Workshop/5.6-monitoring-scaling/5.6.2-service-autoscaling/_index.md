---
title: "ECS Service Auto Scaling"
date: 2026-09-24
weight: 2
chapter: false
pre: " <b> 5.6.2. </b> "
---

# 5.6.2. Configuring ECS Service Auto Scaling Based on Load

Alert emails are good, but it's even better if the system can **automatically handle** high loads instead of waking us up at midnight. We will configure ECS to automatically spin up more Containers when CPU hits 70%.

### Step 1: Enable Auto Scaling on the ECS Service

1. Navigate to the **ECS** service and open the `Eshop-ECS-Cluster`.
2. In the **Services** tab, check the box next to `Eshop-Backend-Service` and click the **Update** button.
3. Scroll down to the **Service auto scaling** section and check **Use service auto scaling**.
4. Fill in the scaling parameters:
   - **Minimum number of tasks**: `2` (Always maintain at least 2 containers for high availability).
   - **Maximum number of tasks**: `10` (Prevent runaway costs by setting an upper limit).
5. Under **Scaling policies**:
   - Policy type: Select **Target tracking**.
   - Policy name: `Scale-Out-High-CPU`.
   - ECS service metric: Select **ECSServiceAverageCPUUtilization**.
   - **Target value**: `70` (When the cluster's average CPU exceeds 70%, ECS adds new Containers to bring the average down; conversely, it scales in when traffic drops).
   - Scale-out cooldown period: `60` (Wait 60 seconds between scale-out actions).
   - Scale-in cooldown period: `60` (Wait 60 seconds between scale-in actions).
6. Scroll to the bottom and click **Update**.

![Configure ECS Target Tracking Policy](/images/5-Workshop/5.6.2/ecs_target_tracking.png)

### The Chain Reaction Mechanism

Your system is now a perfect automated machine:
1. A surge of customers hits the site -> Container CPU rises to 75%.
2. **ECS Service Auto Scaling** detects it's above the 70% target and orders the creation of a 3rd Container.
3. If the current 2 EC2 instances are out of RAM/CPU and can't fit the 3rd Container, the **ECS Capacity Provider** sends an SOS.
4. The **EC2 Auto Scaling Group** receives the signal and immediately spins up a new EC2 virtual server.
5. Once the new EC2 boots up, the 3rd Container is placed on it -> Load drops back to a safe level.

Everything happens completely automatically!