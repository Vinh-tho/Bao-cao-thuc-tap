---
title: "Tạo S3 Bucket Frontend"
date: 2026-09-24
weight: 1
chapter: false
pre: " <b> 3.3.1. </b> "
---

# 3.3.1. Khởi tạo S3 Bucket cho Frontend & Cấu hình Static Website Hosting

Amazon S3 Bucket này sẽ đóng vai trò như một máy chủ web tĩnh (Static Web Server), chịu trách nhiệm phân phối giao diện người dùng (Frontend) khi có truy cập vào hệ thống E-shop.

### Bước 1: Khởi tạo S3 Bucket

1. Tại thanh tìm kiếm trên giao diện AWS Management Console, nhập và chọn dịch vụ **S3**.
2. Nhấn nút **Create bucket**.
3. Cấu hình các thông số cơ bản:
   - **Bucket name**: Nhập `eshop-frontend-<tên-định-danh>` (Lưu ý: Tên Bucket trên hệ thống S3 yêu cầu tính duy nhất trên toàn cầu, định dạng chữ thường và không chứa khoảng trắng).
   - **AWS Region**: Chọn `ap-southeast-1 (Singapore)`.
4. Tại phần **Object Ownership**, chọn `ACLs disabled (recommended)`.

![Khởi tạo S3 Bucket cho Frontend](/images/3-Workshop/3.3/3.3.1/Screenshot%202026-09-25%20221141.png)
![Khởi tạo S3 Bucket cho Frontend](/images/3-Workshop/3.3/3.3.1/Screenshot%202026-09-25%20221231.png)
![Khởi tạo S3 Bucket cho Frontend](/images/3-Workshop/3.3/3.3.1/Screenshot%202026-09-25%20221743.png)
![Khởi tạo S3 Bucket cho Frontend](/images/3-Workshop/3.3/3.3.1/Screenshot%202026-09-25%20221805.png)

### Bước 2: Cấu hình quyền truy cập công cộng (Public Access)

Để cho phép người dùng cuối truy cập giao diện web, Bucket cần được cấu hình mở quyền truy cập mạng công cộng.

1. Cuộn xuống phần **Block Public Access settings for this bucket**.
2. **Bỏ chọn** (Uncheck) tùy chọn `Block all public access`.
3. Đánh dấu vào ô xác nhận *"I acknowledge that the current settings might result in this bucket and the objects within becoming public."* để chấp thuận thay đổi.
4. Cuộn xuống cuối trang và nhấn **Create bucket** để thực thi.

![Tắt Block Public Access](/images/3-Workshop/3.3/3.3.1/Screenshot%202026-09-25%20221957.png)
![Tắt Block Public Access](/images/3-Workshop/3.3/3.3.1/Screenshot%202026-09-25%20222020.png)
![Tắt Block Public Access](/images/3-Workshop/3.3/3.3.1/Screenshot%202026-09-25%20222034.png)

### Bước 3: Kích hoạt tính năng Static Website Hosting

1. Nhấn vào tên Bucket vừa khởi tạo (`eshop-frontend-...`) để truy cập trang chi tiết.
2. Chuyển sang thẻ **Properties** và cuộn xuống phần **Static website hosting**.
3. Nhấn **Edit**.
4. Chọn **Enable**.
5. Cấu hình thông tin các tệp tài liệu:
   - **Index document**: `index.html`
   - **Error document**: `error.html`
6. Nhấn **Save changes** để lưu cấu hình.

![Bật Static Website Hosting](/images/3-Workshop/3.3/3.3.1/Screenshot%202026-09-25%20223242.png)
![Bật Static Website Hosting](/images/3-Workshop/3.3/3.3.1/Screenshot%202026-09-25%20223316.png)
![Bật Static Website Hosting](/images/3-Workshop/3.3/3.3.1/Screenshot%202026-09-25%20223334.png)
![Bật Static Website Hosting](/images/3-Workshop/3.3/3.3.1/Screenshot%202026-09-25%20223358.png)
![Bật Static Website Hosting](/images/3-Workshop/3.3/3.3.1/Screenshot%202026-09-25%20223406.png)
![Bật Static Website Hosting](/images/3-Workshop/3.3/3.3.1/Screenshot%202026-09-25%20223426.png)

### Bước 4: Cấu hình Bucket Policy để cấp quyền truy xuất tài nguyên

Mặc dù đã vô hiệu hóa "Block Public Access", hệ thống vẫn yêu cầu thiết lập chính sách (Policy) nhằm cấp quyền đọc đối tượng (`GetObject`) một cách tường minh.

1. Chuyển sang thẻ **Permissions**.
2. Cuộn xuống phần **Bucket policy**, nhấn **Edit**.
3. Dán đoạn mã JSON dưới đây vào khung soạn thảo (Yêu cầu thay thế `tên-bucket-của-bạn` bằng tên Bucket thực tế đã khởi tạo):
4. Nhấn **Save changes** để hoàn tất.

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "PublicReadGetObject",
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::tên-bucket-của-bạn/*"
        }
    ]
}
```
![Thêm Bucket Policy để cấp quyền đọc file](/images/3-Workshop/3.3/3.3.1/Screenshot%202026-09-25%20223923.png)
![Thêm Bucket Policy để cấp quyền đọc file](/images/3-Workshop/3.3/3.3.1/Screenshot%202026-09-25%20223946.png)
![Thêm Bucket Policy để cấp quyền đọc file](/images/3-Workshop/3.3/3.3.1/Screenshot%202026-09-25%20224038.png)
![Thêm Bucket Policy để cấp quyền đọc file](/images/3-Workshop/3.3/3.3.1/Screenshot%202026-09-25%20224143.png)
![Thêm Bucket Policy để cấp quyền đọc file](/images/3-Workshop/3.3/3.3.1/Screenshot%202026-09-25%20224157.png)
