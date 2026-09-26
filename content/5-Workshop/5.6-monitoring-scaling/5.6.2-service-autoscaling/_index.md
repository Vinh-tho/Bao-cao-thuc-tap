---
title: "ECS Service Auto Scaling"
date: 2026-09-24
weight: 2
chapter: false
pre: " <b> 5.6.2. </b> "
---

# 5.6.2. Configuring ECS Service Auto Scaling Based on Load

This section describes the process of configuring the Auto Scaling feature for the ECS service, aimed at automatically expanding and shrinking compute resources (the number of Tasks/Containers) based on actual load. This configuration helps the system maintain stable performance during sudden traffic spikes, while also automatically optimizing operating costs during low-traffic periods without any manual intervention.

### Step 1: Enable Auto Scaling on the ECS Service

1. Access the **ECS** service on the AWS Console interface and open the `Eshop-ECS-Cluster` cluster.
2. In the **Services** tab, select the `Eshop-Backend-Service` service and click **Update**.
3. Go to the **Service auto scaling** section and check **Use service auto scaling**.
4. Configure the task count limits:
   - **Minimum number of tasks**: `2` (ensures High Availability).
   - **Maximum number of tasks**: `10` (caps the scaling limit to keep the budget under control).
5. In the **Scaling policies** section, set the following parameters:
   - **Policy type**: Select **Target tracking**.
   - **Policy name**: Enter `Scale-Out-High-CPU`.
   - **ECS service metric**: Select **ECSServiceAverageCPUUtilization**.
   - **Target value**: `70` (the system automatically scales out by adding Tasks when average CPU exceeds 70%, and scales in by removing Tasks when the load decreases).
   - **Scale-out cooldown period**: `60` (the wait time between scale-out cycles, in seconds).
   - **Scale-in cooldown period**: `60` (the wait time between scale-in cycles, in seconds).
6. Review the parameters and click **Update** at the bottom of the page to apply the configuration.

![Configuring the ECS Target Tracking Policy](/images/5-Workshop/5.6/5.6.2/Screenshot%202026-09-26%20232703.png)
![Configuring the ECS Target Tracking Policy](/images/5-Workshop/5.6/5.6.2/Screenshot%202026-09-26%20232935.png)
![Configuring the ECS Target Tracking Policy](/images/5-Workshop/5.6/5.6.2/Screenshot%202026-09-26%20232941.png)

### Evaluating the Chain-Reaction Auto Scaling Mechanism

Once the configuration is complete, the system's Auto Scaling architecture will operate fully automatically through the following event flow:
1. An increase in system traffic causes the CPU usage of the existing Containers to exceed the threshold (for example, 75%).
2. **ECS Service Auto Scaling** detects that the monitored metric has exceeded the target value (70%) and immediately triggers the Scale-out policy, issuing a command to launch a 3rd Task (Container).
3. If the EC2 servers currently in the cluster no longer have enough resources (CPU/Memory) to allocate to the new Task, the **ECS Capacity Provider** will detect this resource shortage (capacity exhaustion).
4. This signal triggers the **EC2 Auto Scaling Group (ASG)** to provision a new EC2 virtual server.
5. Once the new EC2 server finishes booting up and joins the cluster, the 3rd Task will be automatically allocated and deployed onto this server, bringing the system's overall average load back down to a safe level.