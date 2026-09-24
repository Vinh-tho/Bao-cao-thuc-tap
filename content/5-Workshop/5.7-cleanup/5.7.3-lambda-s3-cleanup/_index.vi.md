---
title: "Dọn dẹp Lambda & S3"
date: 2026-09-24
weight: 3
chapter: false
pre: " <b> 5.7.3. </b> "
---

# 5.7.3. Dọn dẹp AWS Lambda & Amazon S3 Buckets

Tiếp theo, chúng ta sẽ dọn dẹp các dịch vụ Serverless và kho lưu trữ tĩnh của Frontend cũng như Media.

### Bước 1: Xóa hàm AWS Lambda

1. Truy cập dịch vụ **Lambda** trên AWS Console.
2. Tại menu bên trái, chọn **Functions**.
3. Tick chọn hàm `Eshop-Image-Resizer` mà chúng ta đã tạo ở bài 5.5.1.
4. Nhấn nút **Actions** -> **Delete**. 
5. Gõ chữ `delete` vào ô xác nhận và nhấn nút **Delete**.

![Xóa hàm Lambda](/images/5-Workshop/5.7.3/delete_lambda.png)

### Bước 2: Làm rỗng và Xóa Amazon S3 Buckets

Như đã đề cập, S3 có một cơ chế bảo vệ: Bạn không thể xóa một Bucket nếu bên trong nó vẫn còn chứa dữ liệu.

1. Truy cập dịch vụ **S3**.
2. Tìm đến Bucket chứa Frontend (Ví dụ: `eshop-frontend-...`).
3. Click vào tên Bucket, tick chọn tất cả các file (`index.html`, thư mục `js/`...) bên trong, sau đó nhấn nút **Delete**. Nhập chữ `permanently delete` để xác nhận làm rỗng Bucket.
4. Quay lại danh sách Buckets, chọn `eshop-frontend-...` và nhấn nút **Delete** (để xóa hoàn toàn Bucket khỏi tài khoản).
5. Làm tương tự các bước 2-4 đối với Bucket chứa Media (`eshop-media-...`). Đừng quên xóa cả thư mục `resized/` mà hàm Lambda đã tạo ra trước đó.

![Làm rỗng và xóa S3 Bucket](/images/5-Workshop/5.7.3/delete_s3_bucket.png)

### Bước 3: Xóa CloudWatch Logs (Tùy chọn)

Dù không bắt buộc và tốn rất ít chi phí, việc dọn dẹp log sẽ giúp tài khoản của bạn gọn gàng hơn.
1. Truy cập dịch vụ **CloudWatch**, chọn **Logs** -> **Log groups**.
2. Tìm log group có tên `/aws/lambda/Eshop-Image-Resizer`.
3. Chọn nó, nhấn **Actions** -> **Delete log group** và xác nhận.

Bây giờ hệ thống của bạn chỉ còn lại bộ khung mạng lưới (Network). Ở bài tiếp theo (và cũng là bài cuối cùng), chúng ta sẽ phá dỡ hạ tầng mạng này.