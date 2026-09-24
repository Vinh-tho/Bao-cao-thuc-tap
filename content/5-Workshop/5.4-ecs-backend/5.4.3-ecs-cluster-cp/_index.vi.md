---
title: "Khởi tạo ECS Cluster"
date: 2026-09-24
weight: 3
chapter: false
pre: " <b> 5.4.3. </b> "
---

# 5.4.3. Khởi tạo ECS Cluster (EC2 Launch Type) & ECS Capacity Provider

**Amazon ECS Cluster** là một cụm không gian logic dùng để quản lý các Docker Container. Bằng cách kết nối Cluster này với nhóm Auto Scaling Group (ASG) vừa tạo ở bài 5.4.2 thông qua **Capacity Provider**, ECS có quyền tự động yêu cầu EC2 bật thêm máy chủ mới khi các Container cần thêm tài nguyên (RAM/CPU) để xử lý lượng đơn hàng khổng lồ.

### Bước 1: Khởi tạo ECS Cluster

1. Truy cập dịch vụ **ECS (Elastic Container Service)** trên AWS Console.
2. Tại menu bên trái, chọn **Clusters** và nhấn nút **Create cluster**.
3. Tại mục **Cluster configuration**:
   - **Cluster name**: `Eshop-ECS-Cluster`
4. Tại mục **Infrastructure (Hạ tầng)**:
   - AWS Fargate (Serverless) sẽ được tích chọn theo mặc định. Tuy nhiên, kiến trúc của chúng ta dùng EC2 để tối ưu chi phí theo yêu cầu dự án.
   - Hãy tick chọn thêm ô **Amazon EC2 instances**.
5. Ngay khi bạn tick vào EC2, mục **Auto Scaling group (ASG)** sẽ hiện ra.
   - Chọn `Eshop-ECS-ASG` (Nhóm ASG chúng ta đã tạo ở bài 5.4.2) từ danh sách xổ xuống.
   - Việc chọn trực tiếp ASG ở đây sẽ giúp AWS tự động tạo luôn một **Capacity Provider** cho bạn.
6. Kéo xuống dưới cùng và nhấn **Create**.

![Khởi tạo ECS Cluster kết nối với ASG](/images/5-Workshop/5.4.3/create_ecs_cluster.png)

### Bước 2: Kiểm tra Capacity Provider và EC2 Instances

Sau khi Cluster được tạo thành công (mất khoảng 1-2 phút), chúng ta cần xác nhận xem ECS đã nhận diện được các máy chủ EC2 làm "nhân công" chưa.

1. Nhấp vào tên `Eshop-ECS-Cluster` để vào trang chi tiết.
2. Chuyển sang tab **Infrastructure**.
3. Cuộn xuống phần **Capacity providers**, bạn sẽ thấy một provider mới được tự động tạo (thường có tên giống với tên của ASG, trạng thái là *Active*).
4. Cuộn tiếp xuống phần **Container instances**, bạn sẽ thấy có **2 máy chủ EC2** đang ở trạng thái *Active* (Đây chính là 2 máy chủ do ASG khởi tạo ở bài 5.4.2, nay đã đăng ký thành công vào ECS Cluster).

![Kiểm tra hạ tầng ECS Cluster](/images/5-Workshop/5.4.3/verify_ecs_infrastructure.png)

Hạ tầng cụm máy chủ Backend đã sẵn sàng! Ở bài tiếp theo, chúng ta sẽ thiết lập "Cửa ngõ" Load Balancer để dẫn khách hàng vào cụm máy chủ này.