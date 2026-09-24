---
title: "Định nghĩa ECS Task & Service"
date: 2026-09-24
weight: 5
chapter: false
pre: " <b> 5.4.5. </b> "
---

# 5.4.5. Định nghĩa ECS Task, Khởi tạo Service & Kết nối ALB

Sau khi đã có hạ tầng máy chủ (ECS Cluster) và cửa ngõ giao tiếp (Load Balancer), bước cuối cùng là định nghĩa cách chạy Container của bạn (Task Definition) và ra lệnh cho hệ thống duy trì nó chạy liên tục (Service).

### Bước 1: Tạo Task Definition (Bản thiết kế Container)

1. Truy cập dịch vụ **ECS**, ở menu bên trái chọn **Task definitions** và nhấn **Create new task definition**.
2. **Task definition family**: Đặt tên là `Eshop-Backend-Task`.
3. Tại phần **Infrastructure requirements**:
   - **Launch type**: Chọn **Amazon EC2 instances**.
   - **Network mode**: Chọn **bridge** (Điều này rất quan trọng để ECS có thể tự động gán port ngẫu nhiên (Dynamic Port Mapping) trên EC2 tránh xung đột).
   - **Task size**: Memory = `512`, CPU = `0.5 vCPU`.
   - **Task role & Task execution role**: Chọn `Eshop-ECS-Task-Execution-Role` (Đã tạo ở bài 5.2.1).
4. Tại phần **Container - 1**:
   - **Name**: `eshop-backend-container`
   - **Image URI**: Dán đường dẫn URI của Image bạn đã đẩy lên ECR ở bài 5.4.1 (ví dụ: `123456789.dkr.ecr.ap-southeast-1.amazonaws.com/eshop-backend:latest`).
   - **Port mappings**: 
     - **Container port**: `80` (Hoặc port mà code Backend của bạn đang lắng nghe, ví dụ 8080/3000).
     - **Host port**: Để trống hoặc nhập `0` (Để kích hoạt Dynamic Port Mapping).
     - **Protocol**: `TCP`.
5. Cuộn xuống cuối và nhấn **Create**.

![Tạo ECS Task Definition](/images/5-Workshop/5.4.5/create_task_definition.png)

### Bước 2: Khởi tạo ECS Service và Kết nối Load Balancer

1. Quay lại menu **Clusters**, nhấp vào `Eshop-ECS-Cluster`.
2. Tại tab **Services**, nhấn nút **Create**.
3. **Environment**:
   - Compute options: Chọn **Capacity provider strategy**.
   - Use custom strategy: Chọn Capacity Provider của bạn (ví dụ: `Eshop-ECS-ASG`).
4. **Deployment configuration**:
   - Application type: **Service**.
   - Family: Chọn `Eshop-Backend-Task` (vừa tạo ở Bước 1).
   - Service name: `Eshop-Backend-Service`.
   - Desired tasks (Số lượng Container muốn chạy): `2`.
5. **Networking**: Kéo qua phần này vì chúng ta dùng `bridge` mode.
6. **Load balancing**: 
   - Load balancer type: Chọn **Application Load Balancer**.
   - Load balancer name: Chọn `Eshop-ALB`.
   - Tại mục *Container to load balance*, chọn container `eshop-backend-container`.
   - Target group: Chọn **Use an existing target group** và chọn `Eshop-Backend-TG`.
7. Kéo xuống dưới cùng và nhấn **Create**.

![Khởi tạo ECS Service](/images/5-Workshop/5.4.5/create_ecs_service.png)

### Bước 3: Kiểm tra kết quả

1. Quá trình triển khai Service có thể mất 1-2 phút. Bạn có thể theo dõi tiến trình ở tab **Deployments** và **Tasks** trong Cluster.
2. Khi trạng thái các Task chuyển sang **Running**, hãy quay lại lấy **DNS Name** của ALB (mà bạn đã lưu ở bài 5.4.4).
3. Mở trình duyệt mới, dán đường dẫn DNS của ALB vào và nhấn Enter. 
   - Nếu bạn thấy phản hồi từ API Backend của mình (ví dụ: chuỗi JSON `{"status": "ok", "message": "Backend is running!"}`), **XIN CHÚC MỪNG!** Hệ thống Backend Container của bạn đã hoạt động hoàn hảo và sẵn sàng nhận tải!