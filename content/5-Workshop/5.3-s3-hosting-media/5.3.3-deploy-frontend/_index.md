---
title: "Upload source code to S3"
date: 2026-09-24
weight: 3
chapter: false
pre: " <b> 5.3.3. </b> "
---

# 5.3.3. Upload Frontend source code and static resources to S3

After completing the configuration of the S3 Buckets, the next step is to upload the web interface source code (compiled into static HTML, CSS, and JS formats) to the Frontend Bucket to deploy the application.

### Step 1: Prepare and build the Frontend source code

The entire source code of the project is centrally stored and managed on GitHub at:  
`https://github.com/Vinh-tho/Eshop.git`

The deployment process begins by cloning the source code to the local environment. After navigating into the directory containing the Frontend source code (`eShop.Web`), execute the installation and build commands (`npm install` and `npm run build` or `ng build`). 
The system will automatically optimize, package the entire project, and output the static resources to the `dist` folder. This folder contains the necessary structural and interface files such as `index.html`, `.js` files, `.css` files, and the `assets` folder.

![Prepare and build the Frontend source code](/images/5-Workshop/5.3/5.3.3/Screenshot%202026-09-26%20011303.png)

### Step 2: Upload resources to the S3 Bucket

1. Access the **S3** service on the AWS Management Console and open the Bucket created for the Frontend (specifically `eshop-frontend-eshop-web`).
2. In the **Objects** tab, select the **Upload** button to switch to the data upload interface.
3. On the Upload screen, proceed to upload the static resources (compiled in Step 1) to the system. You can use one of the following two methods:
   * **Method 1 (Drag and drop - Recommended):** Open the folder containing the compiled source code on your computer (the specific path is `dist/e-shop.web/browser`). Select all the files (including `index.html`, `.js`, `.css` files, etc.) and subfolders, then drag and drop them directly into the *"Drag and drop files and folders..."* area on the AWS interface.
   * **Method 2 (Using function buttons):** Click the **Add files** button to select and upload individual files (`index.html`, `.js`, `.css` files). If there are subfolders (e.g., `assets`), continue by clicking the **Add folder** button to upload them.
4. **Technical Requirement:** Ensure that the `index.html` file is uploaded directly to the root directory of the Bucket (not wrapped inside another folder) so that the Static Website Hosting feature can recognize and launch it correctly.
5. Scroll down to the bottom of the page and click the orange **Upload** button to begin the process. Wait for the process to reach 100% completion, then click **Close** to review the list of objects that have appeared in the Bucket.

![Upload Frontend source code to S3 Bucket](/images/5-Workshop/5.3/5.3.3/Screenshot%202026-09-26%20011816.png)
![Upload Frontend source code to S3 Bucket](/images/5-Workshop/5.3/5.3.3/Screenshot%202026-09-26%20011824.png)
![Upload Frontend source code to S3 Bucket](/images/5-Workshop/5.3/5.3.3/Screenshot%202026-09-26%20012427.png)
![Upload Frontend source code to S3 Bucket](/images/5-Workshop/5.3/5.3.3/Screenshot%202026-09-26%20012508.png)
![Upload Frontend source code to S3 Bucket](/images/5-Workshop/5.3/5.3.3/Screenshot%202026-09-26%20012549.png)
![Upload Frontend source code to S3 Bucket](/images/5-Workshop/5.3/5.3.3/Screenshot%202026-09-26%20012620.png)
![Upload Frontend source code to S3 Bucket](/images/5-Workshop/5.3/5.3.3/Screenshot%202026-09-26%20012637.png)
![Upload Frontend source code to S3 Bucket](/images/5-Workshop/5.3/5.3.3/Screenshot%202026-09-26%20012658.png)

### Step 3: Verify website operation

1. On the detail management page of the `eshop-frontend-eshop-web` Bucket, switch to the **Properties** tab.
2. Scroll down to the **Static website hosting** section and click on the URL provided by AWS under **Bucket website endpoint** (e.g., `http://eshop-frontend-eshop-web.s3-website-ap-southeast-1.amazonaws.com`).
3. The browser will navigate to the address above and display the E-shop system interface, confirming that the Frontend deployment process to the S3 service has been successful.

![Access S3 Website Endpoint](/images/5-Workshop/5.3/5.3.3/Screenshot%202026-09-26%20013206.png)
![Access S3 Website Endpoint](/images/5-Workshop/5.3/5.3.3/Screenshot%202026-09-26%20014405.png)
![Access S3 Website Endpoint](/images/5-Workshop/5.3/5.3.3/Screenshot%202026-09-26%20014423.png)