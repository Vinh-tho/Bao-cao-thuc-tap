---
title: "Create Frontend S3 Bucket"
date: 2026-09-24
weight: 1
chapter: false
pre: " <b> 5.3.1. </b> "
---

# 5.3.1. Creating an S3 Bucket for Frontend & Configuring Static Website Hosting

This bucket will act as a static Web Server delivering the UI to users when they visit the E-shop.

### Step 1: Create the S3 Bucket

1. From the search bar on the AWS Console, type **S3** and select the **S3** service.
2. Click the **Create bucket** button.
3. Fill in the basic details:
   - **Bucket name**: `eshop-frontend-<your-name>` (Note: S3 Bucket names must be globally unique, lowercase, with no spaces).
   - **AWS Region**: Select `ap-southeast-1 (Singapore)`.
4. Under **Object Ownership**, select `ACLs disabled (recommended)`.

![Khởi tạo S3 Bucket cho Frontend](/images/5-Workshop/5.3/5.3.1/Screenshot%202026-09-25%20221141.png)
![Khởi tạo S3 Bucket cho Frontend](/images/5-Workshop/5.3/5.3.1/Screenshot%202026-09-25%20221231.png)
![Khởi tạo S3 Bucket cho Frontend](/images/5-Workshop/5.3/5.3.1/Screenshot%202026-09-25%20221743.png)
![Khởi tạo S3 Bucket cho Frontend](/images/5-Workshop/5.3/5.3.1/Screenshot%202026-09-25%20221805.png)

### Step 2: Allow Public Access

Since this is a customer-facing website, we need to allow public access.
1. Scroll down to the **Block Public Access settings for this bucket** section.
2. **Uncheck** the `Block all public access` box.
3. Check the acknowledgment box saying *"I acknowledge that the current settings might result in this bucket and the objects within becoming public."* to confirm.
4. Scroll to the bottom and click **Create bucket**.

![Tắt Block Public Access](/images/5-Workshop/5.3/5.3.1/Screenshot%202026-09-25%20221957.png)
![Tắt Block Public Access](/images/5-Workshop/5.3/5.3.1/Screenshot%202026-09-25%20222020.png)
![Tắt Block Public Access](/images/5-Workshop/5.3/5.3.1/Screenshot%202026-09-25%20222034.png)

### Step 3: Enable Static Website Hosting

1. Click on the name of the Bucket you just created (`eshop-frontend-...`) to open its details.
2. Switch to the **Properties** tab and scroll all the way down to the **Static website hosting** section.
3. Click **Edit**.
4. Select **Enable**.
5. Fill in the index document configurations:
   - **Index document**: `index.html`
   - **Error document**: `error.html`
6. Click **Save changes**.

![Bật Static Website Hosting](/images/5-Workshop/5.3/5.3.1/Screenshot%202026-09-25%20223242.png)
![Bật Static Website Hosting](/images/5-Workshop/5.3/5.3.1/Screenshot%202026-09-25%20223316.png)
![Bật Static Website Hosting](/images/5-Workshop/5.3/5.3.1/Screenshot%202026-09-25%20223334.png)
![Bật Static Website Hosting](/images/5-Workshop/5.3/5.3.1/Screenshot%202026-09-25%20223358.png)
![Bật Static Website Hosting](/images/5-Workshop/5.3/5.3.1/Screenshot%202026-09-25%20223406.png)
![Bật Static Website Hosting](/images/5-Workshop/5.3/5.3.1/Screenshot%202026-09-25%20223426.png)

### Step 4: Add a Bucket Policy to Grant Read Permissions

Even though "Block Public Access" is off, you must write a Policy to explicitly allow everyone to read files (`GetObject`).

1. Switch to the **Permissions** tab.
2. Scroll down to the **Bucket policy** section and click **Edit**.
3. Paste the following JSON code into the editor (Remember to replace `your-bucket-name` with your actual Bucket name):

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "PublicReadGetObject",
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::your-bucket-name/*"
        }
    ]
}
```
![Thêm Bucket Policy để cấp quyền đọc file](/images/5-Workshop/5.3/5.3.1/Screenshot%202026-09-25%20223923.png)
![Thêm Bucket Policy để cấp quyền đọc file](/images/5-Workshop/5.3/5.3.1/Screenshot%202026-09-25%20223946.png)
![Thêm Bucket Policy để cấp quyền đọc file](/images/5-Workshop/5.3/5.3.1/Screenshot%202026-09-25%20224038.png)
![Thêm Bucket Policy để cấp quyền đọc file](/images/5-Workshop/5.3/5.3.1/Screenshot%202026-09-25%20224143.png)
![Thêm Bucket Policy để cấp quyền đọc file](/images/5-Workshop/5.3/5.3.1/Screenshot%202026-09-25%20224157.png)