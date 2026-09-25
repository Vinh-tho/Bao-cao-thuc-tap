---
title: "Tạo S3 Bucket Frontend"
date: 2026-09-24
weight: 1
chapter: false
pre: " <b> 5.3.1. </b> "
---

# 5.3.1. Tạo S3 Bucket cho Frontend & Cấu hình Static Website Hosting

Bucket này sẽ đóng vai trò như một Web Server tĩnh trả về giao diện cho người dùng khi họ truy cập vào E-shop.

### Bước 1: Khởi tạo S3 Bucket

1. Từ thanh tìm kiếm trên AWS Console, gõ **S3** và chọn dịch vụ **S3**.
2. Nhấn nút **Create bucket**.
3. Điền thông tin cơ bản:
   - **Bucket name**: `eshop-frontend-<tên-của-bạn>` (Lưu ý: Tên Bucket trên S3 phải là duy nhất trên toàn cầu, không được trùng với bất kỳ ai, viết thường và không có khoảng trắng).
   - **AWS Region**: Chọn `ap-southeast-1 (Singapore)`.
4. Tại mục **Object Ownership**, chọn `ACLs disabled (recommended)`.

![Khởi tạo S3 Bucket cho Frontend](/images/5-Workshop/5.1/5.1.1/Screenshot%202026-09-25%20181154.png)

### Bước 2: Cho phép truy cập công cộng (Public Access)

Vì đây là web hiển thị cho khách hàng, chúng ta cần mở quyền truy cập công cộng.
1. Cuộn xuống mục **Block Public Access settings for this bucket**.
2. **Bỏ tick** (Uncheck) ô `Block all public access`.
3. Tích vào ô xác nhận *"I acknowledge that the current settings might result in this bucket and the objects within becoming public."* để đồng ý mở khóa.
4. Kéo xuống cuối cùng và nhấn **Create bucket**.

![Tắt Block Public Access](/images/5-Workshop/5.3.1/unblock_public_access.png)

### Bước 3: Bật tính năng Static Website Hosting

1. Nhấn vào tên Bucket bạn vừa tạo (`eshop-frontend-...`) để vào trang chi tiết.
2. Chuyển sang tab **Properties** và cuộn xuống dưới cùng cùng tìm mục **Static website hosting**.
3. Nhấn **Edit**.
4. Chọn **Enable**.
5. Điền thông tin file cấu hình:
   - **Index document**: `index.html`
   - **Error document**: `error.html`
6. Nhấn **Save changes**.

![Bật Static Website Hosting](/images/5-Workshop/5.3.1/enable_static_hosting.png)

### Bước 4: Thêm Bucket Policy để cấp quyền đọc file

Dù đã tắt "Block Public Access", bạn vẫn phải viết luật (Policy) cho phép mọi người đọc file (`GetObject`).

1. Chuyển sang tab **Permissions**.
2. Cuộn xuống mục **Bucket policy**, nhấn **Edit**.
3. Dán đoạn JSON sau vào ô trống (Nhớ thay thế `tên-bucket-của-bạn` bằng tên Bucket thực tế):

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