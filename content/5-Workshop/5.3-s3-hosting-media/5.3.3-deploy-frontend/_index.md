---
title: "Upload Source Code to S3"
date: 2026-09-24
weight: 3
chapter: false
pre: " <b> 5.3.3. </b> "
---

# 5.3.3. Uploading Frontend Source Code and Static Assets to S3

Now that the repositories are ready, we will upload the web interface source code (HTML, CSS, JS) to the Frontend S3 Bucket to make the website live.

### Step 1: Prepare the Frontend Source Code

In a real-world scenario, you would have a build folder containing your source code. If you are following this Workshop, please download the sample source code from the project's GitHub repository:

1. Visit the link: `[https://github.com/YourOrganization/aws-eshop-workshop](https://github.com/YourOrganization/aws-eshop-workshop)` *(Example link)*.
2. Download and extract the source code. You will find a folder named `frontend-dist` containing files like `index.html`, `error.html`, `style.css`, and a `js/` folder.

### Step 2: Upload Files to the Frontend Bucket

1. Open the AWS Console, navigate to the **S3** service, and click on the Bucket you created in section 5.3.1 (e.g., `eshop-frontend-johndoe`).
2. In the **Objects** tab, click the **Upload** button.
3. Click **Add files** to upload individual files (`index.html`, `error.html`, `style.css`).
4. Click **Add folder** to upload the entire `js/` folder (and any other necessary folders).
5. Scroll to the bottom and click the **Upload** button. Wait for the progress bar to reach 100%.
6. Once complete, click **Close** to return to the Objects list.

![Upload Source Code to S3](/images/5-Workshop/5.3.3/upload_frontend_files.png)

### Step 3: Access Your Web E-shop

1. On the Frontend Bucket's details page, switch to the **Properties** tab.
2. Scroll all the way down to the **Static website hosting** section.
3. You will see a link under **Bucket website endpoint** (e.g., `[http://eshop-frontend-...s3-website-ap-southeast-1.amazonaws.com](http://eshop-frontend-...s3-website-ap-southeast-1.amazonaws.com)`).
4. Click that link. Your browser will open and... **Boom!** Your Web E-shop interface is officially live on the Internet.

![Access S3 Website Endpoint](/images/5-Workshop/5.3.3/visit_s3_endpoint.png)