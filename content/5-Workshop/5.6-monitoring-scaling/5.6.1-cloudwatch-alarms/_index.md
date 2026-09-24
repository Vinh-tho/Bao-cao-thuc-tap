---
title: "CloudWatch Logs & Alarms"
date: 2026-09-24
weight: 1
chapter: false
pre: " <b> 5.6.1. </b> "
---

# 5.6.1. Setting Up CloudWatch Logs & Alarms

**Amazon CloudWatch** is AWS's comprehensive monitoring and observability service. We will set up an Alarm so the system automatically sends an email to the administrator if the Backend's CPU utilization exceeds a safe threshold.

### Step 1: Create an Email Topic with SNS (Simple Notification Service)

Before creating an alarm, we need a communication channel to send emails.
1. Navigate to the **SNS** service on the AWS Console.
2. In the left menu, select **Topics** and click **Create topic**.
3. **Type**: Select **Standard**.
4. **Name**: `Eshop-Alert-Topic`. Click **Create topic**.
5. On the details page of the newly created Topic, click the **Subscriptions** tab -> **Create subscription**.
6. **Protocol**: Select **Email**.
7. **Endpoint**: Enter your personal Email address. Click **Create subscription**.
8. Open your email inbox, find the email from "AWS Notifications," and click the **Confirm subscription** link to verify.

![Confirm SNS Email Subscription](/images/5-Workshop/5.6.1/sns_confirm_email.png)

### Step 2: Create a CloudWatch Alarm for CPU

1. Navigate to the **CloudWatch** service.
2. In the left menu, select **All alarms** and click **Create alarm**.
3. Click **Select metric**.
4. Browse to: `ECS` -> `ClusterName, ServiceName`.
5. Find the row where the Cluster name is `Eshop-ECS-Cluster` and Service name is `Eshop-Backend-Service`, select the **CPUUtilization** metric, and click **Select metric**.
6. Under **Conditions**:
   - Threshold type: **Static**.
   - Whenever CPUUtilization is...: Select **Greater/Equal (>=)**.
   - Than...: Enter `80` (Meaning alarm when CPU >= 80%).
7. Click **Next**.
8. Under **Notification**:
   - Select **In alarm**.
   - Choose **Select an existing SNS topic**.
   - Select `Eshop-Alert-Topic` created in Step 1. Click **Next**.
9. **Alarm name**: Name it `Eshop-High-CPU-Alarm`. Click **Next**.
10. Scroll to the bottom and click **Create alarm**.

![Create CloudWatch Alarm for ECS CPU](/images/5-Workshop/5.6.1/create_cloudwatch_alarm.png)

From now on, if the Backend server becomes overloaded, you will receive an immediate email notification!