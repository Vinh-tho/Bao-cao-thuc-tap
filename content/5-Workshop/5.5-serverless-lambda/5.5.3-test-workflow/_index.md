---
title: "Testing the Event Flow"
date: 2026-09-24
weight: 3
chapter: false
pre: " <b> 5.5.3. </b> "
---

# 5.5.3. Testing the Image Upload and Automatic Processing (Resize) Flow

This section describes the process of testing and validating the event-driven processing flow between Amazon S3 and AWS Lambda.

### Step 1: Upload a Sample Object to S3

1. On the **S3** service interface, access the `eshop-media-eshop-web` storage bucket.
2. Go to the **Objects** tab.
3. Select **Upload** and upload a sample image file (for example: `Hinh-nen-Full-HD-1080-cho-may-tinh-dep.jpg`).
4. Click **Upload** and wait for the system to confirm the upload was successful.

![Uploading a Sample Object to S3](/images/5-Workshop/5.5/5.5.3/Screenshot%202026-09-26%20224451.png)

### Step 2: Monitor and Validate the Event Flow on CloudWatch (Audit)

Since the current Lambda source code is set up as mock code for the purpose of testing the integration flow, the system will not physically create a `resized/` folder in S3. Instead, the entire event reception and processing workflow will be recorded in the system logs. The validation process is carried out as follows:

1. Access the **CloudWatch** service on the AWS Console interface.
2. In the left navigation bar, go to the **Logs** section and select **Log Management**.
3. Access the log group named `/aws/lambda/Eshop-Image-Resizer`.
4. Select the most recent log stream just created by the system.
5. Check the log events. A successful execution will record a log sequence confirming that the processing matches the uploaded file:
   - `INFO Event received from S3: {...}`
   - `INFO Processing object: Hinh-nen-Full-HD-1080-cho-may-tinh-dep.jpg from bucket: eshop-media-eshop-web`
   - `INFO Processing complete, new object placed at: resized/Hinh-nen-Full-HD-1080-cho-may-tinh-dep.jpg`

![Viewing Lambda Logs in CloudWatch](/images/5-Workshop/5.5/5.5.3/Screenshot%202026-09-26%20225857.png)
![Viewing Lambda Logs in CloudWatch](/images/5-Workshop/5.5/5.5.3/Screenshot%202026-09-26%20225910.png)

**Conclusion:** Successful validation through the system logs demonstrates the feasibility of the Serverless Event-Driven mechanism. The integration flow between S3 and Lambda is operating stably, ensuring the ability to receive events in real time and is ready for the integration of advanced image-processing libraries (such as Sharp) in the subsequent development phases of the project.