---
title: "Creating the Lambda Function"
date: 2026-09-24
weight: 1
chapter: false
pre: " <b> 5.5.1. </b> "
---

# 5.5.1. Creating the Source Code and AWS Lambda Function for Image Processing (Resize)

This section describes the process of creating an AWS Lambda function to automate image processing. The Lambda function receives an event whenever a new image is uploaded to Amazon S3, resizes it, and saves the result back to the bucket.

### Step 1: Create the Lambda Function

The function setup process on the AWS Console is carried out as follows:
1. Access the **Lambda** service on the AWS Console interface and select **Create function**.
2. Choose the **Author from scratch** method.
3. Set up the basic parameters:
   - **Function name**: `Eshop-Image-Resizer`
   - **Runtime**: Select `Node.js 26.x`.
4. Configure the execution role:
   - Expand the **Additional settings** section.
   - In the **General** field, enable the **Custom execution role** option.
   - Point it to the IAM Role `Eshop-Lambda-Image-Role` (created in section 5.2.1 to grant Lambda read/write permissions on S3 data).
5. Click **Create function** to complete the setup process.

![Creating the Lambda Function](/images/5-Workshop/5.5/5.5.1/Screenshot%202026-09-26%20221447.png)
![Creating the Lambda Function](/images/5-Workshop/5.5/5.5.1/Screenshot%202026-09-26%20222529.png)
![Creating the Lambda Function](/images/5-Workshop/5.5/5.5.1/Screenshot%202026-09-26%20222611.png)

### Step 2: Integrate the Image Processing Source Code

Once the Lambda function has been created, the processing logic source code is configured as follows:
1. On the `Eshop-Image-Resizer` function management interface, go to the **Code source** section.
2. Open the `index.mjs` file in the built-in code editor.
3. Update the source code that handles incoming S3 events, using the AWS SDK v3 library:

```javascript
import { S3Client } from "@aws-sdk/client-s3";

const s3 = new S3Client({ region: "ap-southeast-1" });

export const handler = async (event) => {
    console.log("Event received from S3:", JSON.stringify(event, null, 2));
    
    try {
        const bucket = event.Records[0].s3.bucket.name;
        const key = decodeURIComponent(event.Records[0].s3.object.key.replace(/\+/g, " "));
        
        if (key.startsWith('resized/')) {
            return { status: 'Skipped, image already processed' };
        }

        console.log(`Processing object: ${key} from bucket: ${bucket}`);
        
        const copyKey = `resized/${key}`;
        
        console.log(`Processing complete, new object placed at: ${copyKey}`);
        return { statusCode: 200, body: 'Processing successful!' };
        
    } catch (error) {
        console.error("Error during processing:", error);
        throw error;
    }
};
```