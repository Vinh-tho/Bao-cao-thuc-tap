---
title: "Serverless & Event-Driven"
date: 2026-09-24
weight: 5
chapter: false
pre: " <b> 5.5. </b> "
---

# 5.5. Processing Background Tasks with Event-Driven AWS Lambda

In real-world E-shop systems, when an Admin uploads a new product image (which is often very large), the system needs to generate scaled-down versions (Thumbnails, Medium sizes) to optimize page load speeds for customers.

If we let the main Backend servers (ECS) handle this image processing, it will consume a massive amount of CPU and RAM, potentially causing delays for critical transactions like Checkout or Add to Cart.

The optimal solution is to use an **Event-Driven Serverless** architecture: completely decoupling image processing from the main servers. We will use **AWS Lambda** — a serverless computing service that only runs (and incurs costs) when an event occurs (e.g., a new image is uploaded to S3).

---

### List of detailed practical exercises:

- **[5.5.1. Writing Code & Creating an AWS Lambda Function for Image Resizing](5.5.1-create-lambda/)**
- **[5.5.2. Setting Up S3 Event Notifications to Trigger Lambda](5.5.2-s3-event-trigger/)**
- **[5.5.3. Testing the Image Upload and Auto-Resize Workflow](5.5.3-test-workflow/)**