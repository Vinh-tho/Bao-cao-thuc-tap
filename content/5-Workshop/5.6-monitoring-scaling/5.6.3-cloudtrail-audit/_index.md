---
title: "Auditing with CloudTrail"
date: 2026-09-24
weight: 3
chapter: false
pre: " <b> 5.6.3. </b> "
---

# 5.6.3. Enabling AWS CloudTrail for API Auditing

In an enterprise system architecture, security and the ability to trace the history of operations (audit trail) play a core role. The AWS CloudTrail service is integrated to record all API calls and resource-changing actions on the AWS account, supporting administration, security monitoring, and incident investigation.

### Step 1: Create the CloudTrail

1. Access the **CloudTrail** service on the AWS Console interface.
2. On the main screen, select **Create trail**. The system will redirect to the **Quick trail create** configuration interface.
3. **Trail name**: `Eshop-Audit-Trail`.
4. In the **Trail log bucket and folder** section, the system will automatically create a dedicated S3 bucket to store the log files, following a standard naming format (e.g., `aws-cloudtrail-logs-...`).
5. Review the configuration information and click the **Create trail** button in the bottom-right corner to complete the setup.

![Creating the CloudTrail](/images/5-Workshop/5.6/5.6.3/Screenshot%202026-09-26%20233616.png)
![Creating the CloudTrail](/images/5-Workshop/5.6/5.6.3/Screenshot%202026-09-26%20233802.png)
![Creating the CloudTrail](/images/5-Workshop/5.6/5.6.3/Screenshot%202026-09-26%20234024.png)

### Step 2: Check the Event History

1. In the left navigation bar of CloudTrail, select **Event history**.
2. The system displays a list of all administrative API actions that have been executed on the account recently.
3. Here, observe the event records automatically logged, such as `CreateTrail`, `CreateBucket`, and `StartLogging`, corresponding to the configuration actions just performed.
4. Check the details of the fields recorded by the system, including: the event name (`Event name`), the time of execution (`Event time`), the user identity (`User name`, showing `root`), the service source (`Event source`), and the name of the related resource (`Resource name`).

![Viewing Event History in CloudTrail](/images/5-Workshop/5.6/5.6.3/Screenshot%202026-09-26%20234317.png)

**Conclusion:** Enabling and verifying AWS CloudTrail confirms that the system has successfully and automatically logged every administrative API call. This meets security compliance requirements, making it easy for administrators to track infrastructure changes and ensuring transparency throughout the entire operation of the E-shop project.