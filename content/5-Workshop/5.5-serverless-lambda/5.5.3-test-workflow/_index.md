---
title: "Test Event Workflow"
date: 2026-09-24
weight: 3
chapter: false
pre: " <b> 5.5.3. </b> "
---

# 5.5.3. Testing the Image Upload and Auto-Resize Workflow

It's time to see if our Serverless architecture works flawlessly!

### Step 1: Upload a Sample Image to S3

1. In the **S3** service console, open your Media Bucket (`eshop-media-...`).
2. Switch to the **Objects** tab.
3. Click the **Upload** button and upload any sample image file (e.g., `product-01.jpg`).
4. Click **Upload** and wait for the success message.

### Step 2: Verify the Results on S3

1. Return to the **Objects** list of your Media Bucket and click the circular **Refresh** button.
2. If the Lambda function executed correctly, you should see a newly and automatically created folder named `resized/`.
3. Click into the `resized/` folder, and you will find your processed `product-01.jpg` resting there!

![Image Processing Results on S3](/images/5-Workshop/5.5.3/s3_resized_result.png)

### Step 3: Review Execution Logs on CloudWatch (Audit)

To understand what happened behind the scenes (or to troubleshoot if the `resized/` folder didn't appear), we will check CloudWatch.

1. Navigate to the **CloudWatch** service on the AWS Console.
2. In the left menu, select **Logs** -> **Log groups**.
3. Find and click on the Log group named `/aws/lambda/Eshop-Image-Resizer`.
4. Under the **Log streams** tab, click on the most recent stream (at the top).
5. You will see the detailed log lines (`console.log`) that we wrote in our Lambda source code in section 5.5.1:
   - `Event received from S3: {...}`
   - `Processing image: product-01.jpg from bucket: eshop-media-...`
   - `Processing complete, new file saved at: resized/product-01.jpg`

![Review Lambda Logs on CloudWatch](/images/5-Workshop/5.5.3/cloudwatch_lambda_logs.png)

*🎉 **Excellent!** You have successfully deployed an automated background processing workflow, completely decoupled from your main Backend. This makes your E-shop system much more lightweight and stable.*