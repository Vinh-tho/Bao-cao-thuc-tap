---
title: "Lưu trữ Media trên S3"
date: 2026-09-24
weight: 2
chapter: false
pre: " <b> 3.3.2. </b> "
---

# 3.3.2. Khởi tạo S3 Bucket lưu trữ Media (Hình ảnh sản phẩm)

Trong hệ thống E-shop, hình ảnh sản phẩm yêu cầu khả năng lưu trữ và phân phối với tốc độ cao đến người dùng cuối. Amazon S3 là giải pháp lưu trữ tối ưu đáp ứng yêu cầu này. Ngoài ra, Bucket này sẽ được thiết lập làm nguồn kích hoạt (Trigger) cho hàm AWS Lambda xử lý hình ảnh tại Chương 3.5.

### Bước 1: Khởi tạo S3 Bucket cho Media

1. Tại giao diện dịch vụ **S3**, nhấn nút **Create bucket**.
2. Điền thông tin cơ bản:
   - **Bucket name**: `eshop-media-eshop-web` .
   - **AWS Region**: Chọn `ap-southeast-1 (Singapore)`.
3. Tại mục **Object Ownership**, chọn `ACLs disabled (recommended)`.

![Khởi tạo S3 Bucket cho Media](/images/3-Workshop/3.3/3.3.2/Screenshot%202026-09-25%20225917.png)
![Khởi tạo S3 Bucket cho Media](/images/3-Workshop/3.3/3.3.2/Screenshot%202026-09-25%20230015.png)
![Khởi tạo S3 Bucket cho Media](/images/3-Workshop/3.3/3.3.2/Screenshot%202026-09-25%20230026.png)

### Bước 2: Cấu hình quyền truy cập công cộng (Public Access)

Để hình ảnh sản phẩm có thể hiển thị trực tiếp trên trình duyệt của người dùng, Bucket cần được cấu hình mở quyền truy cập mạng công cộng.

1. Cuộn xuống phần **Block Public Access settings for this bucket**.
2. **Bỏ chọn** (Uncheck) tùy chọn `Block all public access`.
3. Đánh dấu vào ô xác nhận *"I acknowledge that the current settings might result in this bucket and the objects within becoming public."*
4. Cuộn xuống cuối trang và nhấn **Create bucket** để thực thi.

![Mở quyền Public Access cho Media Bucket](/images/3-Workshop/3.3/3.3.2/Screenshot%202026-09-25%20230049.png)
![Mở quyền Public Access cho Media Bucket](/images/3-Workshop/3.3/3.3.2/Screenshot%202026-09-25%20230105.png)
![Mở quyền Public Access cho Media Bucket](/images/3-Workshop/3.3/3.3.2/Screenshot%202026-09-25%20230125.png)

### Bước 3: Cấu hình Bucket Policy để cho phép xem ảnh

1. Mở Bucket `eshop-media-eshop-web` vừa tạo và chuyển sang tab **Permissions**.
2. Kéo xuống phần **Bucket policy** và nhấn **Edit**.
3. Dán đoạn mã JSON sau vào hộp thoại:

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "PublicReadGetObject",
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::eshop-media-eshop-web"
        }
    ]
}
```
4. Nhấn **Save changes**. 

![Cấu hình Bucket Policy để cho phép xem ảnh](/images/3-Workshop/3.3/3.3.2/Screenshot%202026-09-26%20003820.png)
![Cấu hình Bucket Policy để cho phép xem ảnh](/images/3-Workshop/3.3/3.3.2/Screenshot%202026-09-26%20003836.png)
![Cấu hình Bucket Policy để cho phép xem ảnh](/images/3-Workshop/3.3/3.3.2/Screenshot%202026-09-26%20003906.png)
![Cấu hình Bucket Policy để cho phép xem ảnh](/images/3-Workshop/3.3/3.3.2/Screenshot%202026-09-26%20003922.png)
![Cấu hình Bucket Policy để cho phép xem ảnh](/images/3-Workshop/3.3/3.3.2/Screenshot%202026-09-26%20003945.png)

### Bước 4: Cấu hình CORS (Cross-Origin Resource Sharing)

Vì giao diện Frontend (ở Bucket 3.3.1) sẽ gọi ảnh từ Bucket Media này (khác tên miền), cần cấp quyền CORS để trình duyệt không chặn hình ảnh.

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

![Cấu hình CORS cho Media Bucket](/images/3-Workshop/3.3/3.3.2/Screenshot%202026-09-26%20004533.png)
![Cấu hình CORS cho Media Bucket](/images/3-Workshop/3.3/3.3.2/Screenshot%202026-09-26%20004601.png)
![Cấu hình CORS cho Media Bucket](/images/3-Workshop/3.3/3.3.2/Screenshot%202026-09-26%20004617.png)
![Cấu hình CORS cho Media Bucket](/images/3-Workshop/3.3/3.3.2/Screenshot%202026-09-26%20004643.png)

Bây giờ, kho lưu trữ hình ảnh đã sẵn sàng phục vụ cho E-shop!