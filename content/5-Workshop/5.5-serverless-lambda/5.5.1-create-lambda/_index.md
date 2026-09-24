---
title: "Create Lambda Function"
date: 2026-09-24
weight: 1
chapter: false
pre: " <b> 5.5.1. </b> "
---

# 5.5.1. Writing Code & Creating an AWS Lambda Function for Image Resizing

In this lesson, we will create a Lambda function that acts as an "invisible photo editor." Whenever called, it will take the original image, resize it, and save it back to S3.

### Step 1: Initialize the Lambda Function

1. Navigate to the **Lambda** service on the AWS Console and click **Create function**.
2. Select the **Author from scratch** option.
3. Fill in the basic details:
   - **Function name**: `Eshop-Image-Resizer`
   - **Runtime**: Select `Node.js 20.x` (Or Python depending on your provided source code).
4. Expand the **Change default execution role** section:
   - Choose **Use an existing role**.
   - From the dropdown, select `Eshop-Lambda-Image-Role` (Created in section 5.2.1 to grant Lambda permissions to read/write to S3).
5. Click **Create function**.

![Initialize Lambda Function](/images/5-Workshop/5.5.1/create_lambda_function.png)

### Step 2: Add the Image Processing Source Code

1. On the details page of the `Eshop-Image-Resizer` function, scroll down to the **Code source** section.
2. Double-click on the `index.mjs` (or `index.js`) file to open the editor.
3. Paste the mock code or actual image processing code here. Below is a basic S3 event handler framework (using AWS SDK v3):

```javascript
import { S3Client, GetObjectCommand, PutObjectCommand } from "@aws-sdk/client-s3";

const s3 = new S3Client({ region: "ap-southeast-1" });

export const handler = async (event) => {
    console.log("Event received from S3:", JSON.stringify(event, null, 2));
    
    try {
        // 1. Get uploaded file info from the Event
        const bucket = event.Records[0].s3.bucket.name;
        const key = decodeURIComponent(event.Records[0].s3.object.key.replace(/\+/g, " "));
        
        // Prevent infinite loop (only process root files, save to /resized folder)
        if (key.startsWith('resized/')) {
            return { status: 'Skipped, already resized' };
        }

        console.log(`Processing image: ${key} from bucket: ${bucket}`);
        
        // --- ACTUAL IMAGE RESIZING CODE (e.g., using SHARP) GOES HERE ---
        // (For the sake of this workshop, we mock this process)

        const copyKey = `resized/${key}`;
        
        // Report success
        console.log(`Processing complete, new file saved at: ${copyKey}`);
        return { statusCode: 200, body: 'Resize successful!' };
        
    } catch (error) {
        console.error("Processing error:", error);
        throw error;
    }
};
```
4. Click the **Deploy** button to save and apply the code.

![Write Code and Deploy Lambda](/images/5-Workshop/5.5.1/deploy_lambda_code.png)

In the next lesson, we will "tie" this Lambda function to the S3 Media Bucket so it runs automatically upon an event.