---
title: "Kiểm thử luồng Event"
date: 2026-09-24
weight: 3
chapter: false
pre: " <b> 5.5.3. </b> "
---

# 5.5.3. Kiểm thử luồng Upload ảnh và tự động Resize

Đã đến lúc kiểm tra xem kiến trúc Serverless của chúng ta có hoạt động trơn tru hay không!

### Bước 1: Upload một ảnh mẫu lên S3

1. Tại giao diện dịch vụ **S3**, mở Bucket Media của bạn (`eshop-media-...`).
2. Chuyển sang tab **Objects**.
3. Nhấn nút **Upload** và tải lên một file ảnh bất kỳ (ví dụ: `product-01.jpg`).
4. Nhấn **Upload** và đợi thông báo thành công.

### Bước 2: Kiểm tra kết quả trên S3

1. Quay trở lại danh sách **Objects** của Bucket Media và nhấn nút **Refresh** (Làm mới) hình mũi tên xoay vòng.
2. Nếu hàm Lambda hoạt động đúng, bạn sẽ thấy một thư mục mới có tên là `resized/` tự động xuất hiện.
3. Nhấp vào thư mục `resized/`, bạn sẽ thấy file ảnh `product-01.jpg` đã được xử lý và lưu tại đây!

![Kết quả xử lý ảnh trên S3](/images/5-Workshop/5.5.3/s3_resized_result.png)

### Bước 3: Xem Log thực thi trên CloudWatch (Audit)

Để hiểu rõ chuyện gì đã xảy ra ở hậu trường (hoặc để dò lỗi nếu thư mục `resized/` không xuất hiện), chúng ta sẽ kiểm tra CloudWatch.

1. Truy cập dịch vụ **CloudWatch** trên AWS Console.
2. Ở menu bên trái, chọn **Logs** -> **Log groups**.
3. Tìm Log group có tên `/aws/lambda/Eshop-Image-Resizer` và nhấp vào đó.
4. Trong tab **Log streams**, nhấp vào luồng log gần nhất (trên cùng).
5. Bạn sẽ thấy chi tiết các dòng chữ (console.log) mà chúng ta đã viết trong mã nguồn Lambda ở bài 5.5.1:
   - `Event nhận được từ S3: {...}`
   - `Đang xử lý ảnh: product-01.jpg từ bucket: eshop-media-...`
   - `Đã xử lý xong, file mới được lưu tại: resized/product-01.jpg`

![Xem Log Lambda trên CloudWatch](/images/5-Workshop/5.5.3/cloudwatch_lambda_logs.png)

*🎉 **Tuyệt vời!** Bạn đã triển khai thành công một luồng xử lý tác vụ nền tự động, tách biệt hoàn toàn khỏi Backend chính. Việc này giúp hệ thống E-shop của bạn nhẹ nhàng và ổn định hơn rất nhiều.*