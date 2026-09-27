---
title: "Dọn dẹp ECS & ECR"
date: 2026-09-24
weight: 2
chapter: false
pre: " <b> 3.7.2. </b> "
---

# 3.7.2. Dọn dẹp ECS Cluster, Task Definitions & Amazon ECR

Sau khi các máy chủ ảo (EC2) đã được tự động chấm dứt bởi Auto Scaling Group (ASG), quy trình tiếp theo yêu cầu dọn dẹp các cấu hình logic quản lý Container trên dịch vụ ECS và kho lưu trữ Docker Image trên ECR.

### Bước 1: Xóa ECS Service và Cluster

1. Truy cập dịch vụ **ECS (Elastic Container Service)** trên AWS Management Console.
2. Tại thanh điều hướng bên trái, chọn **Clusters** và nhấp vào cụm `Eshop-ECS-Cluster`.
3. Tại thẻ **Services**, chọn `Eshop-Backend-Service` và nhấn nút **Delete**. Nhập từ khóa `delete` để xác nhận. Quá trình xóa Service sẽ mất một khoảng thời gian để hệ thống thu hồi (drain) các tác vụ đang chạy.
4. Sau khi Service đã được gỡ bỏ hoàn toàn khỏi danh sách, nhấn nút **Delete cluster** ở góc trên bên phải. Nhập tên cụm `Eshop-ECS-Cluster` để xác nhận và nhấn **Delete**.

### Bước 2: Hủy đăng ký (Deregister) Task Definitions

1. Tại thanh điều hướng bên trái của giao diện ECS, chọn **Task definitions**.
2. Chọn `Eshop-Backend-Task`.
3. Đánh dấu chọn tất cả các phiên bản (revisions) đang ở trạng thái *Active*.
4. Nhấn chọn menu **Actions** và chọn **Deregister**. (Lưu ý: Hệ thống AWS không hỗ trợ xóa vĩnh viễn Task Definition ngay lập tức; các bản ghi này sẽ được chuyển sang trạng thái Inactive nhằm mục đích lưu vết lịch sử).

### Bước 3: Xóa Amazon ECR Repository

1. Truy cập dịch vụ **Amazon ECR** (Elastic Container Registry).
2. Tại thanh điều hướng bên trái, chọn **Repositories**.
3. Đánh dấu chọn Repository có tên `eshop-backend`.
4. Nhấn nút **Delete** ở góc trên bên phải.
5. Nhập từ khóa `delete` vào ô xác nhận và nhấn **Delete** để thực thi.

*(Lưu ý: Quy trình dọn dẹp các tài nguyên thuộc phân hệ Backend Container đã hoàn tất. Phần tiếp theo sẽ hướng dẫn gỡ bỏ các dịch vụ Serverless bao gồm hàm AWS Lambda và hệ thống lưu trữ S3).*