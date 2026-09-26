---
title: "Khởi tạo Hàm Lambda"
date: 2026-09-24
weight: 1
chapter: false
pre: " <b> 5.5.1. </b> "
---

# 5.5.1. Khởi tạo mã nguồn và hàm AWS Lambda xử lý ảnh (Resize)

Mục này trình bày quy trình khởi tạo hàm AWS Lambda nhằm mục đích tự động hóa việc xử lý hình ảnh. Hàm Lambda đóng vai trò tiếp nhận sự kiện khi có hình ảnh mới được tải lên Amazon S3, thực hiện thu nhỏ kích thước (resize) và lưu trữ lại kết quả vào bucket.

### Bước 1: Khởi tạo hàm Lambda

Quá trình thiết lập hàm trên AWS Console được thực hiện theo các bước sau:
1. Truy cập dịch vụ **Lambda** trên giao diện AWS Console và chọn **Create function**.
2. Lựa chọn phương thức **Author from scratch** (Khởi tạo từ đầu).
3. Thiết lập các thông số cơ bản:
   - **Function name**: `Eshop-Image-Resizer`
   - **Runtime**: Lựa chọn `Node.js 26.x`.
4. Cấu hình quyền thực thi (Execution role):
   - Mở rộng phần **Additional settings**.
   - Tại mục **General**, kích hoạt tùy chọn **Custom execution role**.
   - Trỏ tới IAM Role `Eshop-Lambda-Image-Role` (đã được khởi tạo tại mục 5.2.1 nhằm cấp quyền đọc/ghi dữ liệu S3 cho Lambda).
5. Nhấn **Create function** để hoàn tất quá trình khởi tạo.

![Khởi tạo Hàm Lambda](/images/5-Workshop/5.5/5.5.1/Screenshot%202026-09-26%20221447.png)
![Khởi tạo Hàm Lambda](/images/5-Workshop/5.5/5.5.1/Screenshot%202026-09-26%20222529.png)
![Khởi tạo Hàm Lambda](/images/5-Workshop/5.5/5.5.1/Screenshot%202026-09-26%20222611.png)

### Bước 2: Tích hợp mã nguồn xử lý ảnh

Sau khi hàm Lambda được khởi tạo, mã nguồn xử lý logic được cấu hình như sau:
1. Tại giao diện quản lý hàm `Eshop-Image-Resizer`, di chuyển đến phần **Code source**.
2. Mở tệp `index.mjs` trên trình soạn thảo tích hợp.
3. Cập nhật đoạn mã nguồn thực thi tiếp nhận sự kiện từ S3 sử dụng thư viện AWS SDK v3:

```javascript
import { S3Client } from "@aws-sdk/client-s3";

const s3 = new S3Client({ region: "ap-southeast-1" });

export const handler = async (event) => {
    console.log("Sự kiện nhận được từ S3:", JSON.stringify(event, null, 2));
    
    try {
        const bucket = event.Records[0].s3.bucket.name;
        const key = decodeURIComponent(event.Records[0].s3.object.key.replace(/\+/g, " "));
        
        if (key.startsWith('resized/')) {
            return { status: 'Bỏ qua, ảnh đã được xử lý' };
        }

        console.log(`Đang xử lý đối tượng: ${key} từ bucket: ${bucket}`);
        
        const copyKey = `resized/${key}`;
        
        console.log(`Xử lý hoàn tất, đối tượng mới được quy hoạch tại: ${copyKey}`);
        return { statusCode: 200, body: 'Xử lý thành công!' };
        
    } catch (error) {
        console.error("Lỗi trong quá trình xử lý:", error);
        throw error;
    }
};