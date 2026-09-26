---
title: "Thiết lập S3 Event Trigger"
date: 2026-09-24
weight: 2
chapter: false
pre: " <b> 5.5.2. </b> "
---

# 5.5.2. Thiết lập S3 Event Notification kích hoạt Lambda

Mục này trình bày quy trình thiết lập Event Notification trên Amazon S3. Cấu hình này nhằm mục đích tạo cơ chế kích hoạt (trigger) tự động theo mô hình hướng sự kiện (Event-Driven): mỗi khi có đối tượng hình ảnh mới được tải lên bucket, hệ thống S3 sẽ phát sinh sự kiện để gọi (invoke) hàm Lambda thực thi quy trình xử lý.

### Bước 1: Truy cập cấu hình sự kiện của S3 Bucket

1. Truy cập dịch vụ **S3** trên giao diện AWS Console.
2. Chọn Bucket lưu trữ tài nguyên có tên: `eshop-media-eshop-web`.
3. Di chuyển đến thẻ **Properties** (Thuộc tính).
4. Tại khu vực **Event notifications**, chọn **Create event notification**.

![Truy cập cấu hình sự kiện của S3 Bucket](/images/5-Workshop/5.5/5.5.2/Screenshot%202026-09-26%20223649.png)

### Bước 2: Cấu hình điều kiện kích hoạt sự kiện

1. **Event name**: Nhập tên nhận diện, ví dụ `Trigger-Image-Resize`.
2. **Prefix** (Tiền tố): Bỏ trống (áp dụng cho toàn bộ bucket).
3. **Suffix** (Hậu tố): Bỏ trống (hoặc cấu hình cụ thể `.jpg`, `.png` nếu chỉ muốn giới hạn định dạng tệp tin kích hoạt).
4. Tại mục **Event types**, tích chọn **All object create events** (Kích hoạt khi có bất kỳ đối tượng nào được tạo mới hoặc tải lên).

![Cấu hình Event Types trên S3](/images/5-Workshop/5.5/5.5.2/Screenshot%202026-09-26%20223803.png)

### Bước 3: Cấu hình đích đến (Destination)

1. Di chuyển xuống khu vực **Destination** ở cuối trang.
2. Lựa chọn loại đích đến là **Lambda function**.
3. Tại mục *Specify Lambda function*, chọn tùy chọn **Choose from your Lambda functions**.
4. Chọn hàm `Eshop-Image-Resizer` (đã khởi tạo tại mục 5.5.1) từ danh sách thả xuống.
5. Nhấn **Save changes** để hoàn tất và áp dụng cấu hình.

![Chọn đích đến là Lambda](/images/5-Workshop/5.5/5.5.2/Screenshot%202026-09-26%20223838.png)
![Chọn đích đến là Lambda](/images/5-Workshop/5.5/5.5.2/Screenshot%202026-09-26%20223853.png)

> **Ghi chú kỹ thuật:** Khi thực hiện lưu cấu hình thông qua AWS Console, hệ thống sẽ tự động thiết lập một *Resource-based policy* trên hàm Lambda, cấp quyền `lambda:InvokeFunction` cho dịch vụ S3. Tại bước này, luồng tích hợp tự động giữa S3 và Lambda đã được thiết lập hoàn chỉnh.