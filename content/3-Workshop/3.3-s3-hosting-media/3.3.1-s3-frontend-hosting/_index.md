---
title: "Creating the S3 Frontend Bucket"
date: 2026-09-24
weight: 1
chapter: false
pre: " <b> 3.3.1. </b> "
---

# 3.3.1. Creating an S3 Bucket for the Frontend & Configuring Static Website Hosting

This Amazon S3 Bucket will act as a Static Web Server, responsible for delivering the user interface (Frontend) whenever the E-shop system is accessed.

### Step 1: Create the S3 Bucket

1. In the search bar of the AWS Management Console interface, enter and select the **S3** service.
2. Click the **Create bucket** button.
3. Configure the basic parameters:
   - **Bucket name**: Enter `eshop-frontend-<your-identifier>` (Note: S3 Bucket names must be globally unique, in lowercase, and contain no spaces).
   - **AWS Region**: Select `ap-southeast-1 (Singapore)`.
4. In the **Object Ownership** section, select `ACLs disabled (recommended)`.

![Creating the S3 Bucket for the Frontend](/images/3-Workshop/3.3/3.3.1/Screenshot%202026-09-25%20221141.png)
![Creating the S3 Bucket for the Frontend](/images/3-Workshop/3.3/3.3.1/Screenshot%202026-09-25%20221231.png)
![Creating the S3 Bucket for the Frontend](/images/3-Workshop/3.3/3.3.1/Screenshot%202026-09-25%20221743.png)
![Creating the S3 Bucket for the Frontend](/images/3-Workshop/3.3/3.3.1/Screenshot%202026-09-25%20221805.png)

### Step 2: Configure Public Access Permissions

To allow end users to access the web interface, the Bucket needs to be configured to allow public network access.

1. Scroll down to the **Block Public Access settings for this bucket** section.
2. **Uncheck** the `Block all public access` option.
3. Check the confirmation box *"I acknowledge that the current settings might result in this bucket and the objects within becoming public."* to accept the change.
4. Scroll to the bottom of the page and click **Create bucket** to proceed.

![Disabling Block Public Access](/images/3-Workshop/3.3/3.3.1/Screenshot%202026-09-25%20221957.png)
![Disabling Block Public Access](/images/3-Workshop/3.3/3.3.1/Screenshot%202026-09-25%20222020.png)
![Disabling Block Public Access](/images/3-Workshop/3.3/3.3.1/Screenshot%202026-09-25%20222034.png)

### Step 3: Enable Static Website Hosting

1. Click on the name of the newly created Bucket (`eshop-frontend-...`) to open its detail page.
2. Switch to the **Properties** tab and scroll down to the **Static website hosting** section.
3. Click **Edit**.
4. Select **Enable**.
5. Configure the document file information:
   - **Index document**: `index.html`
   - **Error document**: `error.html`
6. Click **Save changes** to save the configuration.

![Enabling Static Website Hosting](/images/3-Workshop/3.3/3.3.1/Screenshot%202026-09-25%20223242.png)
![Enabling Static Website Hosting](/images/3-Workshop/3.3/3.3.1/Screenshot%202026-09-25%20223316.png)
![Enabling Static Website Hosting](/images/3-Workshop/3.3/3.3.1/Screenshot%202026-09-25%20223334.png)
![Enabling Static Website Hosting](/images/3-Workshop/3.3/3.3.1/Screenshot%202026-09-25%20223358.png)
![Enabling Static Website Hosting](/images/3-Workshop/3.3/3.3.1/Screenshot%202026-09-25%20223406.png)
![Enabling Static Website Hosting](/images/3-Workshop/3.3/3.3.1/Screenshot%202026-09-25%20223426.png)

### Step 4: Configure the Bucket Policy to Grant Resource Access Permissions

Even though "Block Public Access" has been disabled, the system still requires an explicit policy to be set up in order to grant object read (`GetObject`) permissions.

1. Switch to the **Permissions** tab.
2. Scroll down to the **Bucket policy** section and click **Edit**.
3. Paste the following JSON code into the editor (make sure to replace `your-bucket-name` with the actual name of the Bucket you created):
4. Click **Save changes** to finish.

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
![Adding a Bucket Policy to Grant File Read Access](/images/3-Workshop/3.3/3.3.1/Screenshot%202026-09-25%20223923.png)
![Adding a Bucket Policy to Grant File Read Access](/images/3-Workshop/3.3/3.3.1/Screenshot%202026-09-25%20223946.png)
![Adding a Bucket Policy to Grant File Read Access](/images/3-Workshop/3.3/3.3.1/Screenshot%202026-09-25%20224038.png)
![Adding a Bucket Policy to Grant File Read Access](/images/3-Workshop/3.3/3.3.1/Screenshot%202026-09-25%20224143.png)
![Adding a Bucket Policy to Grant File Read Access](/images/3-Workshop/3.3/3.3.1/Screenshot%202026-09-25%20224157.png)