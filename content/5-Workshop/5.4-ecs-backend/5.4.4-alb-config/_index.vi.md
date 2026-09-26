---
title: "Cấu hình ALB & Target Group"
date: 2026-09-26
weight: 4
chapter: false
pre: " <b> 5.4.4. </b> "
---

# 5.4.4. Cấu hình Application Load Balancer (ALB) & Target Group

Application Load Balancer (ALB) đóng vai trò là cửa ngõ duy nhất tiếp nhận các luồng truy cập từ người dùng và phân phối đều tải trọng xuống các Container đang chạy trên cụm máy chủ ECS. Để ALB có thể phân phối luồng dữ liệu chính xác, một Target Group (Nhóm đích) cần được khởi tạo trước.

### Bước 1: Khởi tạo Target Group (Nhóm đích)

1. Truy cập dịch vụ **EC2**, cuộn xuống menu bên trái tìm phần **Load Balancing** và chọn **Target Groups**.
2. Nhấn nút **Create target group**.
3. Tại phần **Basic configuration**:
   - **Choose a target type**: Chọn **Instances**. (Do sử dụng chế độ mạng `bridge` trên ECS, Amazon ECS sẽ tự động đăng ký các máy chủ EC2 vào nhóm này cùng với các port ngẫu nhiên).
   - **Target group name**: Nhập `Eshop-Backend-TG`.
   - **Protocol**: `HTTP`.
   - **Port**: `80`.
   - **VPC**: Chọn **`Eshop-VPC`** (mạng ảo của dự án).
4. Tại phần **Health checks** (Kiểm tra sức khỏe):
   - **Health check protocol**: `HTTP`.
   - **Health check path**: `/` (Hoặc đường dẫn API health check của Backend nếu có).
5. Nhấn **Next** để chuyển sang bước tiếp theo.
6. Tại màn hình *Register targets*, bỏ qua việc chọn máy chủ (ECS Service sẽ tự động thực hiện việc đăng ký này ở phần 5.4.5).
7. Cuộn xuống cuối trang và nhấn **Create target group**.

![Khởi tạo Target Group](/images/5-Workshop/5.4/5.4.4/Screenshot%202026-09-26%20092628.png)
![Khởi tạo Target Group](/images/5-Workshop/5.4/5.4.4/Screenshot%202026-09-26%20093045.png)
![Khởi tạo Target Group](/images/5-Workshop/5.4/5.4.4/Screenshot%202026-09-26%20093110.png)
![Khởi tạo Target Group](/images/5-Workshop/5.4/5.4.4/Screenshot%202026-09-26%20093205.png)

### Bước 2: Khởi tạo Application Load Balancer (ALB)

1. Tại menu bên trái của màn hình EC2, chọn **Load Balancers**.
2. Nhấn nút **Create load balancer**.
3. Dưới mục *Application Load Balancer*, nhấn **Create**.
4. Khai báo phần **Basic configuration**:
   - **Load balancer name**: Nhập `Eshop-ALB`.
   - **Scheme**: Chọn **Internet-facing** (Cho phép ALB tiếp nhận luồng truy cập từ Internet).
   - **IP address type**: Chọn **IPv4**.
5. Tại phần **Network mapping**:
   - **VPC**: Chọn đúng **`Eshop-VPC`**.
   - **Mappings**: Đánh dấu chọn vào ít nhất **2 Availability Zones (AZ)** và chọn các **Public Subnet** tương ứng để đảm bảo tính dự phòng (High Availability).
6. Tại phần **Security groups**:
   - Xóa bỏ nhóm bảo mật `default` mặc định của hệ thống.
   - Chỉ định nhóm bảo mật **`Eshop-ALB-SG`** (nhóm này đã được cấu hình mở cổng `80` cho luồng truy cập từ bên ngoài).
7. Tại phần **Listeners and routing**:
   - **Protocol**: `HTTP`.
   - **Port**: `80`.
   - Tại mục **Default action** (Forward to): Mở danh sách thả xuống và chọn Target Group **`Eshop-Backend-TG`** đã khởi tạo ở Bước 1.
8. Cuộn xuống cuối trang, kiểm tra lại mục *Summary* và nhấn **Create load balancer**.

![Khởi tạo Application Load Balancer](/images/5-Workshop/5.4/5.4.4/Screenshot%202026-09-26%20093523.png)
![Khởi tạo Application Load Balancer](/images/5-Workshop/5.4/5.4.4/Screenshot%202026-09-26%20093620.png)
![Khởi tạo Application Load Balancer](/images/5-Workshop/5.4/5.4.4/Screenshot%202026-09-26%20093738.png)
![Khởi tạo Application Load Balancer](/images/5-Workshop/5.4/5.4.4/Screenshot%202026-09-26%20094031.png)
![Khởi tạo Application Load Balancer](/images/5-Workshop/5.4/5.4.4/Screenshot%202026-09-26%20094239.png)
![Khởi tạo Application Load Balancer](/images/5-Workshop/5.4/5.4.4/Screenshot%202026-09-26%20094407.png)

Quá trình khởi tạo ALB sẽ mất khoảng 2-3 phút. Trạng thái (State) của ALB ban đầu sẽ là *Provisioning*, sau khi chuyển sang **Active** là hoàn tất. Sau bước này, tiến trình triển khai sẽ tiếp tục với Phần 5.4.5 (Khởi tạo ECS Service).