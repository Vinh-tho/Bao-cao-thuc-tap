---
title: "Lưu trữ Media trên S3"
date: 2026-09-24
weight: 2
chapter: false
pre: " <b> 5.3.2. </b> "
---

# 5.3.2. Tạo S3 Bucket lưu trữ Media (Hình ảnh sản phẩm)

Trong hệ thống E-shop, hình ảnh sản phẩm cần được tải lên nhanh chóng và hiển thị mượt mà cho khách hàng. S3 là lựa chọn hoàn hảo cho việc này. Hơn nữa, chúng ta sẽ dùng Bucket này làm nguồn kích hoạt (Trigger) cho hàm AWS Lambda xử lý ảnh ở Chương 5.5.

### Bước 1: Khởi tạo S3 Bucket cho Media

1. Tại giao diện dịch vụ **S3**, nhấn nút **Create bucket**.
2. Điền thông tin cơ bản:
   - **Bucket name**: `eshop-media-<tên-của-bạn>` (Nhớ thay `<tên-của-bạn>` bằng tên duy nhất viết thường, không dấu, không khoảng trắng).
   - **AWS Region**: Chọn `ap-southeast-1 (Singapore)`.
3. Tại mục **Object Ownership**, chọn `ACLs disabled (recommended)`.

![Khởi tạo S3 Bucket cho Media](/images/5-Workshop/5.3/5.3.2/Screenshot%202026-09-25%20225917.png)
![Khởi tạo S3 Bucket cho Media](/images/5-Workshop/5.3/5.3.2/Screenshot%202026-09-25%20230015.png)
![Khởi tạo S3 Bucket cho Media](/images/5-Workshop/5.3/5.3.2/Screenshot%202026-09-25%20230026.png)

### Bước 2: Cho phép truy cập công cộng (Public Access)

Vì hình ảnh sản phẩm cần được khách hàng nhìn thấy trên trình duyệt, chúng ta phải mở quyền truy cập công cộng.

1. Cuộn xuống mục **Block Public Access settings for this bucket**.
2. **Bỏ tick** ô `Block all public access`.
3. Tích vào ô xác nhận *"I acknowledge that the current settings might result in this bucket and the objects within becoming public."*
4. Cuộn xuống dưới cùng và nhấn **Create bucket**.

![Mở quyền Public Access cho Media Bucket](/images/5-Workshop/5.3/5.3.2/Screenshot%202026-09-25%20230049.png)
![Mở quyền Public Access cho Media Bucket](/images/5-Workshop/5.3/5.3.2/Screenshot%202026-09-25%20230105.png)
![Mở quyền Public Access cho Media Bucket](/images/5-Workshop/5.3/5.3.2/Screenshot%202026-09-25%20230125.png)

### Bước 3: Cấu hình Bucket Policy để cho phép xem ảnh

1. Mở Bucket `eshop-media-...` bạn vừa tạo và chuyển sang tab **Permissions**.
2. Kéo xuống phần **Bucket policy** và nhấn **Edit**.
3. Dán đoạn mã JSON sau vào hộp thoại (Lưu ý: Thay thế `tên-bucket-media-của-bạn` bằng tên Bucket thực tế của bạn):

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "PublicReadGetObject",
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::tên-bucket-media-của-bạn/*"
        }
    ]
}
```
4. Nhấn **Save changes**. 

![Cấu hình Bucket Policy để cho phép xem ảnh](/images/5-Workshop/5.3/5.3.2/Screenshot%202026-09-26%20003820.png)
![Cấu hình Bucket Policy để cho phép xem ảnh](/images/5-Workshop/5.3/5.3.2/Screenshot%202026-09-26%20003836.png)
![Cấu hình Bucket Policy để cho phép xem ảnh](/images/5-Workshop/5.3/5.3.2/Screenshot%202026-09-26%20003906.png)
![Cấu hình Bucket Policy để cho phép xem ảnh](/images/5-Workshop/5.3/5.3.2/Screenshot%202026-09-26%20003922.png)
![Cấu hình Bucket Policy để cho phép xem ảnh](/images/5-Workshop/5.3/5.3.2/Screenshot%202026-09-26%20003945.png)

### Bước 4: Cấu hình CORS (Cross-Origin Resource Sharing)

Vì giao diện Frontend (ở Bucket 5.3.1) sẽ gọi ảnh từ Bucket Media này (khác tên miền), chúng ta cần cấp quyền CORS để trình duyệt không chặn hình ảnh.

1. Vẫn ở tab **Permissions**, cuộn xuống dưới cùng tìm mục **Cross-origin resource sharing (CORS)** và nhấn **Edit**.
2. Dán đoạn mã JSON sau vào:

```json
[
    {
        "AllowedHeaders": [
            "*"
        ],
        "AllowedMethods": [
            "GET",
            "HEAD"
        ],
        "AllowedOrigins": [
            "*"
        ],
        "ExposeHeaders": []
    }
]
```
3. Nhấn **Save changes**. 

![Cấu hình CORS cho Media Bucket](/images/5-Workshop/5.3/5.3.2/Screenshot%202026-09-26%20004533.png)
![Cấu hình CORS cho Media Bucket](/images/5-Workshop/5.3/5.3.2/Screenshot%202026-09-26%20004601.png)
![Cấu hình CORS cho Media Bucket](/images/5-Workshop/5.3/5.3.2/Screenshot%202026-09-26%20004617.png)
![Cấu hình CORS cho Media Bucket](/images/5-Workshop/5.3/5.3.2/Screenshot%202026-09-26%20004643.png)

Bây giờ, kho lưu trữ hình ảnh của bạn đã sẵn sàng phục vụ cho E-shop!