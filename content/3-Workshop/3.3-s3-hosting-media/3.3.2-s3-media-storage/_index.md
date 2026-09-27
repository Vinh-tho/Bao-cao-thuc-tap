---
title: "Storing Media on S3"
date: 2026-09-24
weight: 2
chapter: false
pre: " <b> 3.3.2. </b> "
---

# 3.3.2. Creating an S3 Bucket for Media Storage (Product Images)

In the E-shop system, product images need to be stored and delivered to end users at high speed. Amazon S3 is the optimal storage solution for meeting this requirement. In addition, this Bucket will be set up as the trigger source for the AWS Lambda image-processing function covered in Chapter 3.5.

### Step 1: Create the S3 Bucket for Media

1. On the **S3** service interface, click the **Create bucket** button.
2. Fill in the basic information:
   - **Bucket name**: `eshop-media-eshop-web`.
   - **AWS Region**: Select `ap-southeast-1 (Singapore)`.
3. In the **Object Ownership** section, select `ACLs disabled (recommended)`.

![Creating the S3 Bucket for Media](/images/3-Workshop/3.3/3.3.2/Screenshot%202026-09-25%20225917.png)
![Creating the S3 Bucket for Media](/images/3-Workshop/3.3/3.3.2/Screenshot%202026-09-25%20230015.png)
![Creating the S3 Bucket for Media](/images/3-Workshop/3.3/3.3.2/Screenshot%202026-09-25%20230026.png)

### Step 2: Configure Public Access Permissions

In order for product images to be displayed directly in users' browsers, the Bucket needs to be configured to allow public network access.

1. Scroll down to the **Block Public Access settings for this bucket** section.
2. **Uncheck** the `Block all public access` option.
3. Check the confirmation box *"I acknowledge that the current settings might result in this bucket and the objects within becoming public."*
4. Scroll to the bottom of the page and click **Create bucket** to proceed.

![Enabling Public Access for the Media Bucket](/images/3-Workshop/3.3/3.3.2/Screenshot%202026-09-25%20230049.png)
![Enabling Public Access for the Media Bucket](/images/3-Workshop/3.3/3.3.2/Screenshot%202026-09-25%20230105.png)
![Enabling Public Access for the Media Bucket](/images/3-Workshop/3.3/3.3.2/Screenshot%202026-09-25%20230125.png)

### Step 3: Configure the Bucket Policy to Allow Viewing Images

1. Open the `eshop-media-eshop-web` Bucket you just created and switch to the **Permissions** tab.
2. Scroll down to the **Bucket policy** section and click **Edit**.
3. Paste the following JSON code into the dialog box:

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "PublicReadGetObject",
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::eshop-media-eshop-web"
        }
    ]
}
```
4. Click **Save changes**.

![Configuring the Bucket Policy to Allow Viewing Images](/images/3-Workshop/3.3/3.3.2/Screenshot%202026-09-26%20003820.png)
![Configuring the Bucket Policy to Allow Viewing Images](/images/3-Workshop/3.3/3.3.2/Screenshot%202026-09-26%20003836.png)
![Configuring the Bucket Policy to Allow Viewing Images](/images/3-Workshop/3.3/3.3.2/Screenshot%202026-09-26%20003906.png)
![Configuring the Bucket Policy to Allow Viewing Images](/images/3-Workshop/3.3/3.3.2/Screenshot%202026-09-26%20003922.png)
![Configuring the Bucket Policy to Allow Viewing Images](/images/3-Workshop/3.3/3.3.2/Screenshot%202026-09-26%20003945.png)

### Step 4: Configure CORS (Cross-Origin Resource Sharing)

Since the Frontend interface (in Bucket 3.3.1) will call images from this Media Bucket (a different domain), CORS permissions need to be granted so the browser doesn't block the images.

1. Still on the **Permissions** tab, scroll to the bottom to find the **Cross-origin resource sharing (CORS)** section and click **Edit**.
2. Paste the following JSON code:

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

![Configuring CORS for the Media Bucket](/images/3-Workshop/3.3/3.3.2/Screenshot%202026-09-26%20004533.png)
![Configuring CORS for the Media Bucket](/images/3-Workshop/3.3/3.3.2/Screenshot%202026-09-26%20004601.png)
![Configuring CORS for the Media Bucket](/images/3-Workshop/3.3/3.3.2/Screenshot%202026-09-26%20004617.png)
![Configuring CORS for the Media Bucket](/images/3-Workshop/3.3/3.3.2/Screenshot%202026-09-26%20004643.png)

The media storage is now ready to serve the E-shop!