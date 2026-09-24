---
title: "Media Storage on S3"
date: 2026-09-24
weight: 2
chapter: false
pre: " <b> 5.3.2. </b> "
---

# 5.3.2. Creating an S3 Bucket for Media Storage (Product Images)

In an E-shop system, product images must be uploaded quickly and displayed smoothly to customers. S3 is the perfect choice for this. Furthermore, we will use this Bucket as an Event Trigger for our image processing AWS Lambda function in Chapter 5.5.

### Step 1: Create the S3 Bucket for Media

1. In the **S3** service console, click the **Create bucket** button.
2. Fill in the basic details:
   - **Bucket name**: `eshop-media-<your-name>` (Replace `<your-name>` to make it globally unique, lowercase, no spaces).
   - **AWS Region**: Select `ap-southeast-1 (Singapore)`.
3. Under **Object Ownership**, select `ACLs disabled (recommended)`.

![Create S3 Bucket for Media](/images/5-Workshop/5.3.2/create_media_bucket.png)

### Step 2: Allow Public Access

Since product images need to be visible to customers on their browsers, we must open public access.

1. Scroll down to the **Block Public Access settings for this bucket** section.
2. **Uncheck** the `Block all public access` box.
3. Check the acknowledgment box saying *"I acknowledge that the current settings might result in this bucket and the objects within becoming public."*
4. Scroll to the bottom and click **Create bucket**.

![Unblock Public Access for Media Bucket](/images/5-Workshop/5.3.2/unblock_public_access_media.png)

### Step 3: Configure Bucket Policy for Image Viewing

1. Open the `eshop-media-...` Bucket you just created and switch to the **Permissions** tab.
2. Scroll down to the **Bucket policy** section and click **Edit**.
3. Paste the following JSON code into the editor (Note: Replace `your-media-bucket-name` with your actual Bucket name):

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "PublicReadGetObject",
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::your-media-bucket-name/*"
        }
    ]
}
```
4. Click **Save changes**.

### Step 4: Configure CORS (Cross-Origin Resource Sharing)

Because the Frontend UI (hosted in the Bucket from 5.3.1) will request images from this Media Bucket (which is on a different domain), we need to set up CORS permissions so browsers don't block the images.

1. Still in the **Permissions** tab, scroll all the way down to the **Cross-origin resource sharing (CORS)** section and click **Edit**.
2. Paste the following JSON configuration:

```json
[
    {
        "AllowedHeaders": [
            "*"
        ],
        "AllowedMethods": [
            "GET",
            "HEAD"
        ],
        "AllowedOrigins": [
            "*"
        ],
        "ExposeHeaders": []
    }
]
```
3. Click **Save changes**.

![Configure CORS for Media Bucket](/images/5-Workshop/5.3.2/configure_cors_media.png)

Now, your image repository is fully ready to serve the E-shop!