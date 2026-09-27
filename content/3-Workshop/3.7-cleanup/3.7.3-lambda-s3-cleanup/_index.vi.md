---
title: "Dọn dẹp Lambda & S3"
date: 2026-09-24
weight: 3
chapter: false
pre: " <b> 3.7.3. </b> "
---

# 3.7.3. Dọn dẹp AWS Lambda & Amazon S3 Buckets

Tiếp theo, quy trình sẽ tiến hành gỡ bỏ các dịch vụ Serverless và hệ thống kho lưu trữ tĩnh dành cho Frontend cũng như Media.

### Bước 1: Xóa hàm AWS Lambda

1. Truy cập dịch vụ **Lambda** trên giao diện AWS Management Console.
2. Tại thanh điều hướng bên trái, chọn **Functions**.
3. Đánh dấu chọn hàm `Eshop-Image-Resizer` đã được khởi tạo trong các phần trước.
4. Nhấn chọn menu **Actions** và chọn **Delete**. 
5. Nhập từ khóa `delete` vào ô xác nhận và nhấn nút **Delete** để thực thi.

### Bước 2: Làm rỗng và xóa Amazon S3 Buckets

Theo cơ chế bảo vệ của Amazon S3, một Bucket không thể bị xóa nếu vẫn còn chứa đối tượng (dữ liệu) bên trong. Do đó, việc làm rỗng Bucket là thao tác bắt buộc trước khi xóa.

1. Truy cập dịch vụ **S3**.
2. Tìm và truy cập vào Bucket chứa Frontend (Ví dụ: `eshop-frontend-...`).
3. Đánh dấu chọn toàn bộ các tệp tin (`index.html`, thư mục `js/`...) bên trong, sau đó nhấn nút **Delete**. Nhập cụm từ `permanently delete` để xác nhận làm rỗng Bucket.
4. Quay lại danh sách Buckets, chọn Bucket `eshop-frontend-...` vừa làm rỗng và nhấn nút **Delete** để gỡ bỏ hoàn toàn Bucket khỏi tài khoản.
5. Thực hiện lặp lại các thao tác từ 2 đến 4 đối với Bucket lưu trữ Media (`eshop-media-...`). Cần đảm bảo xóa toàn bộ dữ liệu, bao gồm cả thư mục `resized/` do hàm Lambda tự động tạo ra trước đó.

### Bước 3: Xóa CloudWatch Logs (Tùy chọn)

Mặc dù thao tác này không bắt buộc và chi phí duy trì log là rất thấp, việc dọn dẹp log sẽ giúp môi trường tài nguyên được quản lý gọn gàng và tối ưu hơn.
1. Truy cập dịch vụ **CloudWatch**, tại thanh điều hướng chọn **Logs** -> **Log groups**.
2. Tìm kiếm log group có tên `/aws/lambda/Eshop-Image-Resizer`.
3. Đánh dấu chọn log group này, nhấn **Actions** -> **Delete log group** và xác nhận để hoàn tất.

*(Lưu ý: Hiện tại, hệ thống chỉ còn lại bộ khung hạ tầng mạng (Network). Phần tiếp theo, đồng thời là phần cuối cùng, sẽ hướng dẫn quy trình gỡ bỏ toàn bộ kiến trúc mạng này).*