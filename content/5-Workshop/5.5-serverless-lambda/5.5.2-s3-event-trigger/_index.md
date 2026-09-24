---
title: "Set up S3 Event Trigger"
date: 2026-09-24
weight: 2
chapter: false
pre: " <b> 5.5.2. </b> "
---

# 5.5.2. Setting Up S3 Event Notifications to Trigger Lambda

Our `Eshop-Image-Resizer` Lambda function is ready, but it is currently "sleeping." We need to set up an alarm system on the **S3 Media Bucket** so that whenever a new file is uploaded, S3 will automatically wake up the Lambda function and pass the data for processing.

### Step 1: Access S3 Bucket Event Configurations

1. Open the AWS Console and navigate to the **S3** service.
2. Click on the Media Bucket you created in section 5.3.2 (e.g., `eshop-media-johndoe`).
3. Switch to the **Properties** tab.
4. Scroll down to the **Event notifications** section and click the **Create event notification** button.

### Step 2: Configure the Event

1. **Event name**: Enter `Trigger-Image-Resize`.
2. **Prefix** (Optional): You can leave this blank.
3. **Suffix** (Optional): Enter `.jpg` or `.png` if you only want to trigger the function for specific image formats. We can leave it blank to catch all uploads.
4. Under **Event types**, check the box for **All object create events** (Triggers when any new file is created/uploaded to the Bucket).

![Configure S3 Event Types](/images/5-Workshop/5.5.2/s3_event_types.png)

### Step 3: Specify the Destination

1. Scroll all the way down to the **Destination** section.
2. Select **Lambda function**.
3. Under *Specify Lambda function*, select **Choose from your Lambda functions**.
4. Choose the `Eshop-Image-Resizer` function (created in section 5.5.1) from the dropdown list.
5. Click **Save changes**.

![Select Lambda as Destination](/images/5-Workshop/5.5.2/s3_event_destination.png)

> **Note:** When you save this configuration via the AWS Console, S3 automatically adds a *Resource-based policy* to your Lambda function, granting S3 the permission to invoke it.

Done! S3 and Lambda are now tightly coupled into a complete Event-Driven workflow.