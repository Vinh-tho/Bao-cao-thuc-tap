title: "Creating Frontend S3 Bucket"
date: 2026-09-24
weight: 1
chapter: false
pre: " <b> 3.3.1. </b> "
---

# 3.3.1. Creating the S3 Bucket for Frontend & Configuring Static Website Hosting

This Amazon S3 Bucket will act as a Static Web Server, responsible for delivering the user interface (Frontend) when users access the E-shop system.

### Step 1: Creating the S3 Bucket

1. In the search bar on the AWS Management Console, enter and select the **S3** service.
2. Click the **Create bucket** button.
3. Configure the basic parameters:
   - **Bucket name**: Enter `eshop-frontend-<identifier>` (Note: S3 Bucket names must be globally unique, lowercase, and contain no spaces).
   - **AWS Region**: Select `ap-southeast-1 (Singapore)`.
4. Under the **Object Ownership** section, select `ACLs disabled (recommended)`.

![Creating S3 Bucket for Frontend](/images/3-Workshop/3.3/3.3.1/Screenshot%202026-09-25%20221141.png)
![Creating S3 Bucket for Frontend](/images/3-Workshop/3.3/3.3.1/Screenshot%202026-09-25%20221231.png)
![Creating S3 Bucket for Frontend](/images/3-Workshop/3.3/3.3.1/Screenshot%202026-09-25%20221743.png)
![Creating S3 Bucket for Frontend](/images/3-Workshop/3.3/3.3.1/Screenshot%202026-09-25%20221805.png)

### Step 2: Configuring Public Access

To allow end-users to access the web interface, the Bucket must be configured to allow public network access.

1. Scroll down to the **Block Public Access settings for this bucket** section.
2. **Uncheck** the `Block all public access` option.
3. Check the confirmation box *"I acknowledge that the current settings might result in this bucket and the objects within becoming public."* to approve the change.
4. Scroll to the bottom of the page and click **Create bucket** to execute.

![Disabling Block Public Access](/images/3-Workshop/3.3/3.3.1/Screenshot%202026-09-25%20221957.png)
![Disabling Block Public Access](/images/3-Workshop/3.3/3.3.1/Screenshot%202026-09-25%20222020.png)
![Disabling Block Public Access](/images/3-Workshop/3.3/3.3.1/Screenshot%202026-09-25%20222034.png)

### Step 3: Enabling Static Website Hosting

1. Click on the newly created Bucket name (`eshop-frontend-...`) to access the details page.
2. Navigate to the **Properties** tab and scroll down to the **Static website hosting** section.
3. Click **Edit**.
4. Select **Enable**.
5. Configure the document information:
   - **Index document**: `index.html`
   - **Error document**: `error.html`
6. Click **Save changes** to apply the configuration.

![Enabling Static Website Hosting](/images/3-Workshop/3.3/3.3.1/Screenshot%202026-09-25%20223242.png)
![Enabling Static Website Hosting](/images/3-Workshop/3.3/3.3.1/Screenshot%202026-09-25%20223316.png)
![Enabling Static Website Hosting](/images/3-Workshop/3.3/3.3.1/Screenshot%202026-09-25%20223334.png)
![Enabling Static Website Hosting](/images/3-Workshop/3.3/3.3.1/Screenshot%202026-09-25%20223358.png)
![Enabling Static Website Hosting](/images/3-Workshop/3.3/3.3.1/Screenshot%202026-09-25%20223406.png)
![Enabling Static Website Hosting](/images/3-Workshop/3.3/3.3.1/Screenshot%202026-09-25%20223426.png)

### Step 4: Configuring Bucket Policy for Resource Access

Despite disabling "Block Public Access", an explicit policy is required to grant read permissions for objects (`GetObject`).

1. Navigate to the **Permissions** tab.
2. Scroll down to the **Bucket policy** section and click **Edit**.
3. Paste the following JSON code into the editor (Ensure to replace `your-bucket-name` with the actual name of the provisioned Bucket):
4. Click **Save changes** to finalize.

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
![Thêm Bucket Policy để cấp quyền đọc file](/images/3-Workshop/3.3/3.3.1/Screenshot%202026-09-25%20223923.png)
![Thêm Bucket Policy để cấp quyền đọc file](/images/3-Workshop/3.3/3.3.1/Screenshot%202026-09-25%20223946.png)
![Thêm Bucket Policy để cấp quyền đọc file](/images/3-Workshop/3.3/3.3.1/Screenshot%202026-09-25%20224038.png)
![Thêm Bucket Policy để cấp quyền đọc file](/images/3-Workshop/3.3/3.3.1/Screenshot%202026-09-25%20224143.png)
![Thêm Bucket Policy để cấp quyền đọc file](/images/3-Workshop/3.3/3.3.1/Screenshot%202026-09-25%20224157.png)