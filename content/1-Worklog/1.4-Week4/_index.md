---
title: "Week 4 Worklog"
date: 2026-08-22
weight: 4
chapter: false
pre: " <b> 1.4. </b> "
---

### Objectives Week 4:

- Learn AWS Lambda and the Serverless / Event-Driven model.
- Build a Lambda function for product-image processing and trigger it from S3.
- Read execution logs on CloudWatch to verify the event flow.

### Tasks to be carried out this week:

| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| Sat – Sun | Study Lambda: execution model, runtimes, handler, event object, execution role, pay-per-invocation pricing. Understand why image resizing should leave the backend. | 22/08/2026 | 23/08/2026 | <https://docs.aws.amazon.com/lambda/latest/dg/> |
| Mon – Tue | Create the function Eshop-Image-Resizer (Node.js) and an IAM Role with AWSLambdaBasicExecutionRole and S3 access; write the handler using AWS SDK v3 and test with a sample S3 event. | 24/08/2026 | 25/08/2026 | <https://cloudjourney.awsstudygroup.com/> |
| Wed – Thu | Configure S3 Event Notification (All object create events) to invoke the function; confirm the resource-based policy for lambda:InvokeFunction. Add a guard that skips keys starting with resized/ to avoid recursive invocation. | 26/08/2026 | 27/08/2026 | <https://cloudjourney.awsstudygroup.com/> |
| Fri | Upload sample images and verify the flow in CloudWatch Logs (/aws/lambda/Eshop-Image-Resizer): event received, object key, target path resized/. | 28/08/2026 | 28/08/2026 | <https://cloudjourney.awsstudygroup.com/> |

### Technical configuration details:

| Component | Configuration / value |
| --- | --- |
| Function | Eshop-Image-Resizer, runtime Node.js, file index.mjs, AWS SDK v3 (@aws-sdk/client-s3), region ap-southeast-1 |
| Execution role | Eshop-Lambda-Image-Role (custom execution role) |
| Media bucket | eshop-media-eshop-web; Block Public Access off; public GetObject policy; CORS allows GET and HEAD from any origin |
| S3 Event Notification | Name Trigger-Image-Resize; prefix and suffix empty; event type All object create events; destination Lambda function Eshop-Image-Resizer |
| Resource-based policy | Added automatically by the Console: S3 may call lambda:InvokeFunction |
| Log group | /aws/lambda/Eshop-Image-Resizer |

### Knowledge gained:

- A Lambda handler receives an event; for S3 the bucket name is in Records[0].s3.bucket.name and the object key in Records[0].s3.object.key.
- Object keys arrive URL-encoded (spaces become +), so the code applies replace(/\+/g, ' ') and decodeURIComponent.
- Recursive-trigger risk: if the function writes into the same bucket that triggers it, it can invoke itself forever; the guard skips keys beginning with resized/.
- Serverless billing: pay only for invocations and duration; offloading image work keeps CPU and RAM of the backend free for checkout and cart operations.
- CORS is required because the frontend (one bucket/origin) loads images from the media bucket (another origin).

### Commands and code used:

**Lambda handler (event-driven flow validation)**

```javascript
import { S3Client } from "@aws-sdk/client-s3";

const s3 = new S3Client({ region: "ap-southeast-1" });

export const handler = async (event) => {
    console.log("Sự kiện nhận được từ S3:", JSON.stringify(event, null, 2));
    try {
        const bucket = event.Records[0].s3.bucket.name;
        const key = decodeURIComponent(event.Records[0].s3.object.key.replace(/\+/g, " "));
        if (key.startsWith("resized/")) {
            return { status: "Bỏ qua, ảnh đã được xử lý" };
        }
        console.log(`Đang xử lý đối tượng: ${key} từ bucket: ${bucket}`);
        const copyKey = `resized/${key}`;
        console.log(`Xử lý hoàn tất, đối tượng mới được quy hoạch tại: ${copyKey}`);
        return { statusCode: 200, body: "Xử lý thành công!" };
    } catch (error) {
        console.error("Lỗi trong quá trình xử lý:", error);
        throw error;
    }
};
```

### Difficulties and solutions:

- Event-driven thinking differs from traditional request/response development. → Followed one event end to end (upload → event JSON → handler → log) to understand each hop.

### Achievements Week 4:

- Understood the benefits and limits of Serverless (no server management, pay only when invoked).
- Built an S3 → Lambda event flow and verified it through CloudWatch Logs.
- Learned to avoid recursive triggers when a function writes into the bucket that triggers it.
- Current limitation: the function validates the event flow and computes the output path but does not yet resize images; Sharp is planned as a next step.

### Deliverables and evidence:

- CloudWatch log lines: event received, object being processed, output path resized/<file>.
- Function currently validates the event flow (mock logic); real resizing with Sharp is future work.

### Plan for next week:

- Learn CloudWatch, CloudTrail and alarms.
