---
title: "API Auditing with CloudTrail"
date: 2026-09-24
weight: 3
chapter: false
pre: " <b> 5.6.3. </b> "
---

# 5.6.3. Enabling AWS CloudTrail for API Auditing

In an enterprise environment, security and auditing capabilities are mandatory. If the system goes down one day because someone accidentally deleted a Security Group or misconfigured the Load Balancer, you need to know exactly **Who did it, when, and from what IP address**. **AWS CloudTrail** acts as the "security camera," recording every API call made within your AWS account.

### Step 1: Create a Trail

1. From the AWS Console, search for and navigate to the **CloudTrail** service.
2. On the main dashboard, click the **Create trail** button.
3. **Trail name**: `Eshop-Audit-Trail`.
4. Under **Storage location**:
   - Select **Create new S3 bucket**. AWS will automatically create a new bucket prefixed with `aws-cloudtrail-logs-...` to store the log files.
   - Uncheck *Log file SSE-KMS encryption* (for simplicity within this Workshop).
5. Scroll down and click **Next**.
6. Under **Choose log events**, keep the default selection of **Management events** (which records operations like creating, modifying, or deleting resources). Click **Next**.
7. Review your settings on the final page and click **Create trail**.

![Create CloudTrail](/images/5-Workshop/5.6.3/create_cloudtrail.png)

### Step 2: Review the Event History

1. In the left navigation menu of CloudTrail, select **Event history**.
2. Here, you will see a list of all recent actions taken in your account.
3. To test it out, open a new tab, go to EC2, and **try creating a dummy Security Group**, or modify a parameter in an Auto Scaling Group.
4. After about 5-10 minutes, return to the CloudTrail Event history page and refresh. You will see a detailed log entry (Event name: `CreateSecurityGroup`, User name: `Your_IAM_User_Name`, Source IP...).

![View Event History on CloudTrail](/images/5-Workshop/5.6.3/cloudtrail_event_history.png)

Enabling CloudTrail is the very first golden standard (Best Practice) that any System Administrator must apply when taking over a new AWS account.