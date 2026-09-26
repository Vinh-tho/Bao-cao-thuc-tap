---
title: "CloudWatch Logs & Alarms"
date: 2026-09-24
weight: 1
chapter: false
pre: " <b> 5.6.1. </b> "
---

# 5.6.1. Setting Up CloudWatch Logs and Alarms

This section describes the process of setting up an automated monitoring and alerting mechanism using Amazon CloudWatch combined with Amazon SNS. The system is configured to automatically send email notifications to the administrator whenever the Backend service's CPU usage exceeds a safe threshold, ensuring a timely response to and handling of incidents.

### Step 1: Create a Notification Channel with Amazon SNS (Simple Notification Service)

The notification channel setup process is carried out as follows:
1. Access the **SNS** service on the AWS Console interface.
2. In the left navigation bar, select **Topics** and click **Create topic**.
3. **Type**: Select **Standard**.
4. **Name**: Enter `Eshop-Alert-Topic`, then click **Create topic**.
5. On the detail page of the newly created Topic, switch to the **Subscriptions** tab and select **Create subscription**.
6. **Protocol**: Select **Email**.
7. **Endpoint**: Provide the administrator's email address to receive alerts, then click **Create subscription**.
8. Access the registered email inbox, open the confirmation notification from "AWS Notifications", and click the **Confirm subscription** link to activate the notification channel.

![Confirming the SNS Alert Email Subscription](/images/5-Workshop/5.6/5.6.1/Screenshot%202026-09-26%20230543.png)
![Confirming the SNS Alert Email Subscription](/images/5-Workshop/5.6/5.6.1/Screenshot%202026-09-26%20230733.png)
![Confirming the SNS Alert Email Subscription](/images/5-Workshop/5.6/5.6.1/Screenshot%202026-09-26%20231302.png)

### Step 2: Set Up a CloudWatch Alarm to Monitor CPU Resources

The resource monitoring flow is configured through CloudWatch as follows:
1. Access the **CloudWatch** service on the AWS Console interface.
2. In the left navigation bar, go to **Alarms** > **All alarms** and click **Create alarm**.
3. Click **Select metric** to set up the monitoring parameter.
4. Navigate along the path: `ECS` > `ClusterName, ServiceName`.
5. Search for the record with ClusterName `Eshop-ECS-Cluster` and ServiceName `Eshop-Backend-Service`. Check the **CPUUtilization** metric and click **Select metric**.
6. In the **Conditions** configuration area:
   - **Threshold type**: Select **Static**.
   - **Whenever CPUUtilization is...**: Select **Greater/Equal (>=)**.
   - **Than...**: Enter `80` (the alarm is triggered when CPU usage reaches 80% or higher).
7. Click **Next** to proceed to the next step.
8. In the **Notification** area:
   - **Alarm state trigger**: Select **In alarm**.
   - Select **Select an existing SNS topic**.
   - In the dropdown list, specify the `Eshop-Alert-Topic` created in Step 1. Click **Next**.
9. **Alarm name**: Enter `Eshop-High-CPU-Alarm` and click **Next**.
10. On the Review page, review the parameters and click **Create alarm** to complete the setup.

![Creating a CloudWatch Alarm for ECS CPU](/images/5-Workshop/5.6/5.6.1/Screenshot%202026-09-26%20231650.png)
![Creating a CloudWatch Alarm for ECS CPU](/images/5-Workshop/5.6/5.6.1/Screenshot%202026-09-26%20231812.png)
![Creating a CloudWatch Alarm for ECS CPU](/images/5-Workshop/5.6/5.6.1/Screenshot%202026-09-26%20231944.png)
![Creating a CloudWatch Alarm for ECS CPU](/images/5-Workshop/5.6/5.6.1/Screenshot%202026-09-26%20232024.png)
![Creating a CloudWatch Alarm for ECS CPU](/images/5-Workshop/5.6/5.6.1/Screenshot%202026-09-26%20232048.png)
![Creating a CloudWatch Alarm for ECS CPU](/images/5-Workshop/5.6/5.6.1/Screenshot%202026-09-26%20232107.png)

**Conclusion:** The monitoring integration has been successfully configured. Whenever the Backend service's CPU usage reaches ≥ 80%, CloudWatch will automatically change its state to "In alarm" and route the notification through SNS to send an alert via email, helping to optimize system operations.