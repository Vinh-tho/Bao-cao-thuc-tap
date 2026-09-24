---
title: "Thiết lập S3 Event Trigger"
date: 2026-09-24
weight: 2
chapter: false
pre: " <b> 5.5.2. </b> "
---

# 5.5.2. Thiết lập S3 Event Notification kích hoạt Lambda

Hàm Lambda `Eshop-Image-Resizer` của chúng ta đã sẵn sàng, nhưng hiện tại nó đang "ngủ". Chúng ta cần thiết lập một hệ thống báo động trên **S3 Media Bucket** để mỗi khi có file mới được tải lên, S3 sẽ tự động đánh thức và truyền dữ liệu cho Lambda xử lý.

### Bước 1: Truy cập cấu hình Event của S3 Bucket

1. Mở AWS Console, truy cập dịch vụ **S3**.
2. Nhấp vào tên Bucket lưu trữ Media mà bạn đã tạo ở bài 5.3.2 (ví dụ: `eshop-media-nguyenvana`).
3. Chuyển sang tab **Properties** (Thuộc tính).
4. Cuộn xuống tìm mục **Event notifications** (Thông báo sự kiện) và nhấn nút **Create event notification**.

### Bước 2: Cấu hình Sự kiện (Event)

1. **Event name**: Nhập `Trigger-Image-Resize`.
2. **Prefix** (Tùy chọn): Bạn có thể để trống.
3. **Suffix** (Tùy chọn): Nhập `.jpg` hoặc `.png` nếu bạn chỉ muốn kích hoạt hàm khi upload các file ảnh cụ thể. Ở đây chúng ta có thể để trống để nhận mọi file.
4. Tại mục **Event types**, tick chọn ô **All object create events** (Kích hoạt khi có bất kỳ file nào được tạo mới/upload lên Bucket).

![Cấu hình Event Types trên S3](/images/5-Workshop/5.5.2/s3_event_types.png)

### Bước 3: Chỉ định Đích đến (Destination)

1. Cuộn xuống dưới cùng tới mục **Destination**.
2. Chọn **Lambda function**.
3. Tại ô *Specify Lambda function*, chọn **Choose from your Lambda functions**.
4. Chọn hàm `Eshop-Image-Resizer` (mà bạn đã tạo ở bài 5.5.1) từ danh sách xổ xuống.
5. Nhấn **Save changes**.

![Chọn đích đến là Lambda](/images/5-Workshop/5.5.2/s3_event_destination.png)

> **Lưu ý:** Khi bạn thao tác lưu cấu hình này trên Console, AWS S3 sẽ tự động thêm một *Resource-based policy* vào hàm Lambda của bạn để cho phép S3 có quyền gọi (invoke) hàm đó.

Xong! Bây giờ S3 và Lambda đã được "trói" chặt vào nhau thành một luồng Event-Driven hoàn chỉnh.