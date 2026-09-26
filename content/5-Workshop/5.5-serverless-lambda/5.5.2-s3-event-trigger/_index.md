---
title: "Setting Up the S3 Event Trigger"
date: 2026-09-24
weight: 2
chapter: false
pre: " <b> 5.5.2. </b> "
---

# 5.5.2. Setting Up an S3 Event Notification to Trigger Lambda

This section describes the process of setting up an Event Notification on Amazon S3. This configuration creates an automatic, event-driven trigger mechanism: whenever a new image object is uploaded to the bucket, S3 generates an event that invokes the Lambda function to carry out the processing workflow.

### Step 1: Access the S3 Bucket's Event Configuration

1. Access the **S3** service on the AWS Console interface.
2. Select the resource storage bucket named: `eshop-media-eshop-web`.
3. Go to the **Properties** tab.
4. In the **Event notifications** section, select **Create event notification**.

![Accessing the S3 Bucket's Event Configuration](/images/5-Workshop/5.5/5.5.2/Screenshot%202026-09-26%20223649.png)

### Step 2: Configure the Event Trigger Conditions

1. **Event name**: Enter an identifying name, for example `Trigger-Image-Resize`.
2. **Prefix**: Leave blank (applies to the entire bucket).
3. **Suffix**: Leave blank (or specify `.jpg`, `.png`, etc. if you want to limit the trigger to specific file formats only).
4. In the **Event types** section, check **All object create events** (triggers whenever any object is newly created or uploaded).

![Configuring Event Types on S3](/images/5-Workshop/5.5/5.5.2/Screenshot%202026-09-26%20223803.png)

### Step 3: Configure the Destination

1. Scroll down to the **Destination** section at the bottom of the page.
2. Select **Lambda function** as the destination type.
3. In the *Specify Lambda function* field, select the **Choose from your Lambda functions** option.
4. Select the `Eshop-Image-Resizer` function (created in section 5.5.1) from the dropdown list.
5. Click **Save changes** to complete and apply the configuration.

![Selecting Lambda as the Destination](/images/5-Workshop/5.5/5.5.2/Screenshot%202026-09-26%20223838.png)
![Selecting Lambda as the Destination](/images/5-Workshop/5.5/5.5.2/Screenshot%202026-09-26%20223853.png)

> **Technical note:** When the configuration is saved through the AWS Console, the system automatically sets up a *resource-based policy* on the Lambda function, granting the `lambda:InvokeFunction` permission to the S3 service. At this point, the automatic integration flow between S3 and Lambda has been fully established.