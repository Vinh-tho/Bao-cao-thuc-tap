---
title: "Dọn dẹp ECS & ECR"
date: 2026-09-24
weight: 2
chapter: false
pre: " <b> 5.7.2. </b> "
---

# 5.7.2. Dọn dẹp ECS Cluster, Task Definitions & Amazon ECR

Sau khi các máy chủ vật lý (EC2) đã bị tắt bởi ASG, chúng ta cần xóa phần cấu hình logic quản lý các Container trên ECS và kho chứa Docker Image.

### Bước 1: Xóa ECS Service và Cluster

1. Truy cập dịch vụ **ECS (Elastic Container Service)**.
2. Tại menu bên trái, chọn **Clusters** và nhấp vào cụm `Eshop-ECS-Cluster`.
3. Tại tab **Services**, chọn `Eshop-Backend-Service` và nhấn nút **Delete**. Nhập chữ `delete` để xác nhận. Việc xóa Service sẽ mất một chút thời gian để hệ thống hủy bỏ (drain) các tác vụ.
4. Sau khi Service đã biến mất khỏi danh sách, nhấn nút **Delete cluster** ở góc trên cùng bên phải. Nhập tên cụm `Eshop-ECS-Cluster` để xác nhận và nhấn **Delete**.

![Xóa ECS Service và Cluster](/images/5-Workshop/5.7.2/delete_ecs_cluster.png)

### Bước 2: Hủy đăng ký (Deregister) Task Definitions

1. Ở menu bên trái của ECS, chọn **Task definitions**.
2. Chọn `Eshop-Backend-Task`.
3. Tick chọn tất cả các phiên bản (revisions) đang ở trạng thái *Active*.
4. Nhấn **Actions** -> **Deregister**. (AWS không cho phép xóa vĩnh viễn Task Definition ngay lập tức mà chỉ chuyển chúng sang trạng thái Inactive để lưu trữ lịch sử).

![Hủy đăng ký Task Definition](/images/5-Workshop/5.7.2/deregister_task_def.png)

### Bước 3: Xóa Amazon ECR Repository

1. Truy cập dịch vụ **Amazon ECR** (Elastic Container Registry).
2. Ở menu bên trái, chọn **Repositories**.
3. Tick chọn Repository có tên `eshop-backend` (nơi chứa Docker Image của bạn).
4. Nhấn nút **Delete** ở góc trên cùng bên phải.
5. Gõ chữ `delete` vào ô xác nhận và nhấn **Delete**.

![Xóa ECR Repository](/images/5-Workshop/5.7.2/delete_ecr_repo.png)

Cụm Backend của bạn đã hoàn toàn bốc hơi. Tiếp theo, chúng ta sẽ dọn dẹp các dịch vụ Serverless bao gồm Lambda xử lý ảnh và S3.