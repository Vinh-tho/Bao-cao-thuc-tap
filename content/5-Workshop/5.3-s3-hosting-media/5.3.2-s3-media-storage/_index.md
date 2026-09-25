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

![Khởi tạo S3 Bucket cho Media](/images/5-Workshop/5.3/5.3.2/Screenshot%202026-09-25%20225917.png)
![Khởi tạo S3 Bucket cho Media](/images/5-Workshop/5.3/5.3.2/Screenshot%202026-09-25%20230015.png)
![Khởi tạo S3 Bucket cho Media](/images/5-Workshop/5.3/5.3.2/Screenshot%202026-09-25%20230026.png)

### Step 2: Allow Public Access

Since product images need to be visible to customers on their browsers, we must open public access.

1. Scroll down to the **Block Public Access settings for this bucket** section.
2. **Uncheck** the `Block all public access` box.
3. Check the acknowledgment box saying *"I acknowledge that the current settings might result in this bucket and the objects within becoming public."*
4. Scroll to the bottom and click **Create bucket**.

![Mở quyền Public Access cho Media Bucket](/images/5-Workshop/5.3/5.3.2/Screenshot%202026-09-25%20230049.png)
![Mở quyền Public Access cho Media Bucket](/images/5-Workshop/5.3/5.3.2/Screenshot%202026-09-25%20230105.png)
![Mở quyền Public Access cho Media Bucket](/images/5-Workshop/5.3/5.3.2/Screenshot%202026-09-25%20230125.png)

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

![Cấu hình Bucket Policy để cho phép xem ảnh](/images/5-Workshop/5.3/5.3.2/Screenshot%202026-09-26%20003820.png)
![Cấu hình Bucket Policy để cho phép xem ảnh](/images/5-Workshop/5.3/5.3.2/Screenshot%202026-09-26%20003836.png)
![Cấu hình Bucket Policy để cho phép xem ảnh](/images/5-Workshop/5.3/5.3.2/Screenshot%202026-09-26%20003906.png)
![Cấu hình Bucket Policy để cho phép xem ảnh](/images/5-Workshop/5.3/5.3.2/Screenshot%202026-09-26%20003922.png)
![Cấu hình Bucket Policy để cho phép xem ảnh](/images/5-Workshop/5.3/5.3.2/Screenshot%202026-09-26%20003945.png)

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

![Cấu hình CORS cho Media Bucket](/images/5-Workshop/5.3/5.3.2/Screenshot%202026-09-26%20004533.png)
![Cấu hình CORS cho Media Bucket](/images/5-Workshop/5.3/5.3.2/Screenshot%202026-09-26%20004601.png)
![Cấu hình CORS cho Media Bucket](/images/5-Workshop/5.3/5.3.2/Screenshot%202026-09-26%20004617.png)
![Cấu hình CORS cho Media Bucket](/images/5-Workshop/5.3/5.3.2/Screenshot%202026-09-26%20004643.png)

Now, your image repository is fully ready to serve the E-shop!