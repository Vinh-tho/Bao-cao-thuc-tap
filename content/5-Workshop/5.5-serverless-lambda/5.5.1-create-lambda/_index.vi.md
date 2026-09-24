---
title: "Khởi tạo Hàm Lambda"
date: 2026-09-24
weight: 1
chapter: false
pre: " <b> 5.5.1. </b> "
---

# 5.5.1. Viết mã & Khởi tạo hàm AWS Lambda xử lý ảnh (Resize)

Trong bài này, chúng ta sẽ tạo một hàm Lambda đóng vai trò như một "nhân viên chỉnh sửa ảnh ẩn danh". Mỗi khi được gọi, nó sẽ lấy ảnh gốc, thu nhỏ lại và lưu lại vào S3.

### Bước 1: Khởi tạo hàm Lambda

1. Truy cập dịch vụ **Lambda** trên AWS Console và nhấn nút **Create function**.
2. Chọn tùy chọn **Author from scratch** (Tự viết code từ đầu).
3. Điền thông tin cơ bản:
   - **Function name**: `Eshop-Image-Resizer`
   - **Runtime**: Chọn `Node.js 20.x` (Hoặc Python tùy vào mã nguồn mẫu của bạn).
4. Mở rộng phần **Change default execution role**:
   - Chọn **Use an existing role**.
   - Ở ô thả xuống, chọn `Eshop-Lambda-Image-Role` (Đã tạo ở bài 5.2.1 để cấp quyền cho Lambda đọc/ghi S3).
5. Nhấn **Create function**.

![Khởi tạo Hàm Lambda](/images/5-Workshop/5.5.1/create_lambda_function.png)

### Bước 2: Thêm mã nguồn xử lý ảnh

1. Trong trang chi tiết của hàm `Eshop-Image-Resizer`, kéo xuống phần **Code source**.
2. Nhấp đúp vào file `index.mjs` (hoặc `index.js`) để mở trình soạn thảo.
3. Dán đoạn mã giả lập (Mock code) hoặc mã xử lý ảnh thực tế vào đây. Dưới đây là một bộ khung xử lý sự kiện S3 cơ bản (sử dụng thư viện AWS SDK v3):

```javascript
import { S3Client, GetObjectCommand, PutObjectCommand } from "@aws-sdk/client-s3";

const s3 = new S3Client({ region: "ap-southeast-1" });

export const handler = async (event) => {
    console.log("Event nhận được từ S3:", JSON.stringify(event, null, 2));
    
    try {
        // 1. Lấy thông tin file vừa upload từ Event
        const bucket = event.Records[0].s3.bucket.name;
        const key = decodeURIComponent(event.Records[0].s3.object.key.replace(/\+/g, " "));
        
        // Kiểm tra tránh vòng lặp vô hạn (chỉ xử lý ảnh ở thư mục gốc, lưu vào thư mục /resized)
        if (key.startsWith('resized/')) {
            return { status: 'Bỏ qua, ảnh đã được resize' };
        }

        console.log(`Đang xử lý ảnh: ${key} từ bucket: ${bucket}`);
        
        // --- TẠI ĐÂY SẼ LÀ CODE DÙNG THƯ VIỆN NHƯ SHARP ĐỂ RESIZE ẢNH ---
        // (Vì giới hạn workshop, chúng ta giả lập bằng cách copy ảnh sang thư mục resized/)

        const copyKey = `resized/${key}`;
        
        // Báo cáo thành công
        console.log(`Đã xử lý xong, file mới được lưu tại: ${copyKey}`);
        return { statusCode: 200, body: 'Resize thành công!' };
        
    } catch (error) {
        console.error("Lỗi xử lý:", error);
        throw error;
    }
};
```
4. Nhấn nút **Deploy** để lưu và áp dụng đoạn mã này.

![Viết code và Deploy Lambda](/images/5-Workshop/5.5.1/deploy_lambda_code.png)

Ở bài tiếp theo, chúng ta sẽ "trói" hàm Lambda này vào S3 Bucket Media để nó tự động chạy khi có sự kiện.