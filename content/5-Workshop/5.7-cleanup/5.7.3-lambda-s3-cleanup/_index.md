---
title: "Cleanup Lambda & S3"
date: 2026-09-24
weight: 3
chapter: false
pre: " <b> 5.7.3. </b> "
---

# 5.7.3. Cleaning up AWS Lambda & Amazon S3 Buckets

Next, we will clean up the Serverless services and the static storage repositories for the Frontend and Media.

### Step 1: Delete AWS Lambda Function

1. Navigate to the **Lambda** service on the AWS Console.
2. In the left menu, select **Functions**.
3. Check the box next to the `Eshop-Image-Resizer` function we created in section 5.5.1.
4. Click **Actions** -> **Delete**.
5. Type `delete` in the confirmation box and click the **Delete** button.

![Delete Lambda Function](/images/5-Workshop/5.7.3/delete_lambda.png)

### Step 2: Empty and Delete Amazon S3 Buckets

As mentioned, S3 has a built-in safeguard: You cannot delete a Bucket if it still contains data.

1. Navigate to the **S3** service.
2. Find your Frontend Bucket (e.g., `eshop-frontend-...`).
3. Click on the Bucket name, select all files (`index.html`, `js/` folder, etc.) inside, and click the **Delete** button. Type `permanently delete` to confirm emptying the bucket.
4. Return to the Buckets list, select `eshop-frontend-...`, and click **Delete** (to completely remove the Bucket from your account).
5. Repeat steps 2-4 for your Media Bucket (`eshop-media-...`). Don't forget to delete the `resized/` folder that the Lambda function previously generated.

![Empty and Delete S3 Bucket](/images/5-Workshop/5.7.3/delete_s3_bucket.png)

### Step 3: Delete CloudWatch Logs (Optional)

While not strictly necessary and incurring minimal costs, cleaning up logs keeps your account tidy.
1. Navigate to the **CloudWatch** service, select **Logs** -> **Log groups**.
2. Find the log group named `/aws/lambda/Eshop-Image-Resizer`.
3. Select it, click **Actions** -> **Delete log group**, and confirm.

Your system is now stripped down to just its network skeleton. In the next (and final) section, we will dismantle this network infrastructure.