---
title: "Định nghĩa ECS Task & Service"
date: 2026-09-24
weight: 5
chapter: false
pre: " <b> 5.4.5. </b> "
---

# 5.4.5. Định nghĩa ECS Task, Khởi tạo Service & Kết nối ALB

Sau khi hoàn thiện hạ tầng máy chủ (ECS Cluster) và bộ cân bằng tải (Load Balancer), bước tiếp theo là định nghĩa cấu hình chạy Container (Task Definition) và thiết lập dịch vụ (ECS Service) nhằm duy trì tính sẵn sàng của ứng dụng.

### Bước 1: Khởi tạo Task Definition (Bản thiết kế Container)

1. Truy cập dịch vụ **ECS**, điều hướng đến **Task definitions** tại menu bên trái và chọn **Create new task definition**.
2. Khai báo thông tin cơ bản:
   - **Task definition family**: `Eshop-Backend-Task`
3. Tại phần **Infrastructure requirements**:
   - **Launch type**: Chỉ chọn **Amazon EC2 instances** (bỏ chọn AWS Fargate).
   - **Network mode**: Chọn **bridge**. (Thiết lập này bắt buộc để kích hoạt tính năng Dynamic Port Mapping trên EC2, giúp ECS tự động gán cổng ngẫu nhiên nhằm tránh xung đột).
   - **Task size**: Nhập thủ công các thông số phù hợp với giới hạn của máy chủ t2.micro: CPU = `0.5 vCPU`, Memory = `0.5 GB`.
   - **Task role & Task execution role**: Chọn `Eshop-ECS-Task-Execution-Role` cho cả hai mục (đã khởi tạo tại phần 5.2.1).
4. Tại phần **Container - 1**:
   - **Name**: `eshop-backend-container`
   - **Image URI**: Nhấn nút **Browse ECR images**. Tại cửa sổ hiện ra, chọn kho lưu trữ `eshop-backend`, tích chọn image có thẻ (tag) là `latest`. Tiếp theo, tại mục *Select image by* ở góc dưới cùng, chọn tùy chọn **Image tag** và nhấn nút **Select image**. Hệ thống sẽ tự động điền đường dẫn URI hoàn chỉnh.
   - **Port mappings**: 
     - **Container port**: `80` (Hoặc cổng dịch vụ Backend đang lắng nghe).
     - **Host port**: Để trống hoặc nhập `0` (Kích hoạt Dynamic Port Mapping).
     - **Protocol**: `TCP`.
5. Cuộn xuống cuối trang và nhấn **Create** để hoàn tất.

![Tạo ECS Task Definition](/images/5-Workshop/5.4/5.4.5/Screenshot%202026-09-26%20072944.png)
![Tạo ECS Task Definition](/images/5-Workshop/5.4/5.4.5/Screenshot%202026-09-26%20073008.png)
![Tạo ECS Task Definition](/images/5-Workshop/5.4/5.4.5/Screenshot%202026-09-26%20073505.png)
![Tạo ECS Task Definition](/images/5-Workshop/5.4/5.4.5/Screenshot%202026-09-26%20073524.png)
![Tạo ECS Task Definition](/images/5-Workshop/5.4/5.4.5/Screenshot%202026-09-26%20074548.png)
![Tạo ECS Task Definition](/images/5-Workshop/5.4/5.4.5/Screenshot%202026-09-26%20074625.png)
![Tạo ECS Task Definition](/images/5-Workshop/5.4/5.4.5/Screenshot%202026-09-26%20074726.png)
![Tạo ECS Task Definition](/images/5-Workshop/5.4/5.4.5/Screenshot%202026-09-26%20074736.png)
![Tạo ECS Task Definition](/images/5-Workshop/5.4/5.4.5/Screenshot%202026-09-26%20074756.png)

### Bước 2: Khởi tạo ECS Service và Tích hợp Load Balancer

1. Tại giao diện **Clusters**, truy cập vào cụm `Eshop-ECS-Cluster`.
2. Chuyển sang thẻ **Services** và chọn **Create**.
3. Tại phần **Service details** (Chi tiết dịch vụ):
   - **Task definition family**: Chọn `Eshop-Backend-Task` (đã thiết lập tại Bước 1).
   - **Service name**: Nhập `Eshop-Backend-Service`.
4. Tại phần **Environment** (Môi trường):
   - Mở rộng mục **Compute configuration - advanced**.
   - **Compute options**: Chọn **Capacity provider strategy**.
   - Chọn **Use custom (Advanced)**.
   - Tại bảng cấu hình, thay đổi Capacity provider từ mặc định (`FARGATE`) sang `Eshop-ECS-CP`.
5. Tại phần **Deployment configuration** (Cấu hình triển khai):
   - **Desired tasks** (Số lượng Task/Container mong muốn duy trì): Nhập `2`.
6. Tại phần **Networking**: Không yêu cầu cấu hình VPC/Subnet do Task đang sử dụng chế độ mạng `bridge`.
7. Tại phần **Load balancing - optional** (Cân bằng tải): 
   - Đánh dấu chọn **Use load balancing**.
   - **VPC**: Chọn đúng mạng **`Eshop-VPC`**.
   - **Load balancer type**: Chọn **Application Load Balancer**.
   - Tại mục **Container**, chọn `eshop-backend-container 80:80`.
   - **Application Load Balancer**: Chọn **Use an existing load balancer**.
   - **Load balancer**: Mở danh sách và chọn **`Eshop-ALB`**.
   - **Listener**: Chọn **Use an existing listener** và đảm bảo cổng **`HTTP:80`** được chọn.
   - **Target group**: Chọn **Use an existing target group** và chỉ định **`Eshop-Backend-TG`**.
8. Cuộn xuống cuối trang và nhấn **Create** để hoàn tất tiến trình khởi tạo.

![Khởi tạo ECS Service](/images/5-Workshop/5.4/5.4.5/Screenshot%202026-09-26%20095119.png)
![Khởi tạo ECS Service](/images/5-Workshop/5.4/5.4.5/Screenshot%202026-09-26%20095234.png)
![Khởi tạo ECS Service](/images/5-Workshop/5.4/5.4.5/Screenshot%202026-09-26%20095255.png)
![Khởi tạo ECS Service](/images/5-Workshop/5.4/5.4.5/Screenshot%202026-09-26%20095416.png)
![Khởi tạo ECS Service](/images/5-Workshop/5.4/5.4.5/Screenshot%202026-09-26%20095837.png)

### Bước 3: Kiểm tra và Xác thực Hệ thống

1. Quá trình triển khai Service cần khoảng 1-2 phút. Tiến trình có thể được giám sát tại thẻ **Deployments** và **Tasks** bên trong giao diện quản lý Cluster.
2. Khi trạng thái của tất cả các Task chuyển sang **Running**, hệ thống đã sẵn sàng tiếp nhận yêu cầu.
3. Mở trình duyệt, truy cập vào địa chỉ **DNS Name** của ALB (được cung cấp từ phần 5.4.5).
   - Việc xác thực thành công khi nhận được phản hồi chính xác từ API Backend (Ví dụ: dữ liệu JSON trả về thông báo trạng thái hoạt động). Hệ thống Backend Container đã được triển khai hoàn chỉnh và có khả năng chịu tải.