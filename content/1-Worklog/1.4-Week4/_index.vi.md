---
title: "Worklog Tuần 4"
date: 2026-08-22
weight: 4
chapter: false
pre: " <b> 1.4. </b> "
---

### Mục tiêu tuần 4:

- Tìm hiểu AWS Lambda và mô hình Serverless / hướng sự kiện.
- Xây dựng hàm Lambda xử lý ảnh sản phẩm và kích hoạt từ S3.
- Đọc log thực thi trên CloudWatch để kiểm chứng luồng sự kiện.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| Thứ 7 – CN | Học Lambda: mô hình thực thi, runtime, handler, event, execution role, tính phí theo lượt gọi. Hiểu vì sao xử lý ảnh nên tách khỏi backend. | 22/08/2026 | 23/08/2026 | <https://docs.aws.amazon.com/lambda/latest/dg/> |
| Thứ 2 – Thứ 3 | Tạo hàm Eshop-Image-Resizer (Node.js) và IAM Role với AWSLambdaBasicExecutionRole cùng quyền S3; viết handler dùng AWS SDK v3 và test bằng sự kiện S3 mẫu. | 24/08/2026 | 25/08/2026 | <https://cloudjourney.awsstudygroup.com/> |
| Thứ 4 – Thứ 5 | Cấu hình S3 Event Notification (All object create events) để gọi hàm; xác nhận resource-based policy cho lambda:InvokeFunction. Thêm điều kiện bỏ qua key bắt đầu bằng resized/ để tránh gọi lặp vô hạn. | 26/08/2026 | 27/08/2026 | <https://cloudjourney.awsstudygroup.com/> |
| Thứ 6 | Upload ảnh mẫu và kiểm chứng luồng trong CloudWatch Logs (/aws/lambda/Eshop-Image-Resizer): nhận sự kiện, object key, đường dẫn đích resized/. | 28/08/2026 | 28/08/2026 | <https://cloudjourney.awsstudygroup.com/> |

### Chi tiết cấu hình kỹ thuật:

| Thành phần | Cấu hình / giá trị |
| --- | --- |
| Hàm Lambda | Eshop-Image-Resizer, runtime Node.js, tệp index.mjs, AWS SDK v3 (@aws-sdk/client-s3), region ap-southeast-1 |
| Execution role | Eshop-Lambda-Image-Role (custom execution role) |
| Bucket media | eshop-media-eshop-web; tắt Block Public Access; policy GetObject công khai; CORS cho phép GET và HEAD từ mọi origin |
| S3 Event Notification | Tên Trigger-Image-Resize; prefix và suffix để trống; event type All object create events; đích là hàm Lambda Eshop-Image-Resizer |
| Resource-based policy | Console tự thêm: S3 được phép gọi lambda:InvokeFunction |
| Log group | /aws/lambda/Eshop-Image-Resizer |

### Kiến thức đã học:

- Handler Lambda nhận một event; với S3, tên bucket nằm ở Records[0].s3.bucket.name và object key ở Records[0].s3.object.key.
- Object key đến ở dạng URL-encoded (khoảng trắng thành +), nên mã dùng replace(/\+/g, ' ') và decodeURIComponent.
- Rủi ro gọi lặp: nếu hàm ghi vào chính bucket đang trigger nó thì có thể tự gọi vô hạn; điều kiện bảo vệ bỏ qua key bắt đầu bằng resized/.
- Tính phí serverless: chỉ trả theo lượt gọi và thời gian chạy; chuyển xử lý ảnh ra Lambda giúp CPU và RAM của backend dành cho giỏ hàng và thanh toán.
- CORS cần thiết vì frontend (một bucket/origin) tải ảnh từ bucket media (origin khác).

### Lệnh và mã đã sử dụng:

**Handler Lambda (kiểm chứng luồng hướng sự kiện)**

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

### Khó khăn và cách giải quyết:

- Tư duy hướng sự kiện khác cách lập trình request/response truyền thống. → Lần theo một sự kiện từ đầu đến cuối (upload → event JSON → handler → log) để hiểu từng chặng.

### Kết quả đạt được tuần 4:

- Hiểu lợi ích và giới hạn của Serverless (không quản lý máy chủ, chỉ trả tiền khi được gọi).
- Xây dựng và kiểm chứng luồng S3 → Lambda qua CloudWatch Logs.
- Biết cách tránh vòng lặp kích hoạt khi hàm ghi ngược vào chính bucket đang trigger.
- Hạn chế hiện tại: hàm mới xác nhận luồng sự kiện và tạo đường dẫn đầu ra, chưa resize ảnh thật; dự kiến dùng Sharp ở giai đoạn sau.

### Sản phẩm và minh chứng:

- Các dòng log CloudWatch: nhận sự kiện, đối tượng đang xử lý, đường dẫn đầu ra resized/<tệp>.
- Hàm hiện kiểm chứng luồng sự kiện (logic mô phỏng); resize ảnh thật bằng Sharp là công việc tiếp theo.

### Kế hoạch tuần tiếp theo:

- Học CloudWatch, CloudTrail và cảnh báo.
