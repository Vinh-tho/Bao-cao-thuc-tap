---
title: "Serverless & Event-Driven"
date: 2026-09-24
weight: 5
chapter: false
pre: " <b> 5.5. </b> "
---

# 5.5. Xử lý Tác vụ Nền bằng Sự kiện (Event-Driven) với AWS Lambda

Trong các hệ thống E-shop thực tế, khi Admin tải lên hình ảnh sản phẩm mới (thường có dung lượng rất lớn), hệ thống cần tạo ra các phiên bản thu nhỏ (Thumbnail, Medium) để tối ưu tốc độ tải trang cho khách hàng.

Nếu để các máy chủ Backend (ECS) làm công việc xử lý ảnh này, nó sẽ ngốn rất nhiều CPU và RAM, gây chậm trễ cho các giao dịch quan trọng khác như Thanh toán hay Thêm vào giỏ hàng. 

Giải pháp tối ưu nhất là sử dụng kiến trúc **Event-Driven Serverless**: Tách biệt hoàn toàn việc xử lý ảnh ra khỏi máy chủ chính. Chúng ta sẽ dùng **AWS Lambda** — một dịch vụ máy tính phi máy chủ, chỉ chạy (và tính tiền) khi có sự kiện xảy ra (có ảnh mới tải lên S3).

---

### Danh sách các bài thực hành chi tiết:

- **[5.5.1. Viết mã & Khởi tạo hàm AWS Lambda xử lý ảnh (Resize)](5.5.1-create-lambda/)**
- **[5.5.2. Thiết lập S3 Event Notification kích hoạt Lambda](5.5.2-s3-event-trigger/)**
- **[5.5.3. Kiểm thử luồng Upload ảnh và tự động Resize](5.5.3-test-workflow/)**