---
title: "Khởi tạo ECS Cluster"
date: 2026-09-24
weight: 3
chapter: false
pre: " <b> 5.4.3. </b> "
---

# 5.4.3. Khởi tạo ECS Cluster (EC2 Launch Type) & ECS Capacity Provider

**Amazon ECS Cluster** là môi trường logic được sử dụng để quản lý và điều phối các Docker Container. Bằng cách tích hợp Cluster này với Auto Scaling Group (ASG) (đã thiết lập tại mục 5.4.2) thông qua **Capacity Provider**, dịch vụ ECS được cấp quyền tự động mở rộng quy mô, yêu cầu EC2 cung cấp thêm máy chủ mới khi các Container cần bổ sung tài nguyên (RAM/CPU) để đáp ứng sự gia tăng đột biến của lưu lượng truy cập.

### Bước 1: Khởi tạo ECS Cluster

Do đặc thù của tài khoản AWS mới thường chưa được khởi tạo sẵn các Service-Linked Role cho ECS, việc khởi tạo Cluster kèm theo Auto Scaling Group ngay từ đầu có thể gây lỗi "Unable to assume the service linked role". Do đó, quy trình được chia làm hai giai đoạn:

1. Truy cập dịch vụ **ECS (Elastic Container Service)** trên giao diện AWS Console.
2. Tại menu điều hướng bên trái, chọn **Clusters** và nhấn **Create cluster**.
3. Tại mục **Cluster configuration**: Khai báo **Cluster name** là `Eshop-ECS-Cluster`.
4. Tại mục **Infrastructure**: Giữ nguyên tùy chọn mặc định **Fargate only** để hệ thống tự động sinh các Role bảo mật cần thiết.
5. Nhấn **Create** để hoàn tất việc tạo cụm cơ bản.

![Khởi tạo ECS Cluster ](/images/5-Workshop/5.4/5.4.3/Screenshot%202026-09-26%20064530.png)
![Khởi tạo ECS Cluster ](/images/5-Workshop/5.4/5.4.3/Screenshot%202026-09-26%20064602.png)
![Khởi tạo ECS Cluster ](/images/5-Workshop/5.4/5.4.3/Screenshot%202026-09-26%20070803.png)
![Khởi tạo ECS Cluster ](/images/5-Workshop/5.4/5.4.3/Screenshot%202026-09-26%20070815.png)
![Khởi tạo ECS Cluster ](/images/5-Workshop/5.4/5.4.3/Screenshot%202026-09-26%20070844.png)

### Bước 2: Tích hợp Auto Scaling Group (Capacity Provider)

1. Truy cập vào trang chi tiết của cụm `Eshop-ECS-Cluster` vừa tạo.
2. Chuyển sang thẻ **Infrastructure**. 
3. Tại phần **Capacity providers**, nhấn chọn **Create**.
4. Khai báo thông tin:
   - **Scaling type**: Chọn **EC2 Auto Scaling** để liên kết với ASG đã tạo thủ công.
   - **Capacity provider name**: Nhập `Eshop-ECS-CP`.
   - **Auto Scaling group**: Chọn `Eshop-ECS-ASG` (nhóm máy chủ đã tạo tại phần 5.4.2).
5. Nhấn **Create** và đợi trạng thái chuyển sang *Active*. Hành động này cấp quyền cho cụm ECS được phép sử dụng các máy chủ EC2 do ASG quản lý.

![Khởi tạo ECS Cluster kết nối với ASG ](/images/5-Workshop/5.4/5.4.3/Screenshot%202026-09-26%20071101.png)
![Khởi tạo ECS Cluster kết nối với ASG ](/images/5-Workshop/5.4/5.4.3/Screenshot%202026-09-26%20071547.png)
![Khởi tạo ECS Cluster kết nối với ASG ](/images/5-Workshop/5.4/5.4.3/Screenshot%202026-09-26%20071619.png)


### Bước 3: Kiểm tra hạ tầng EC2 Instances

1. Vẫn trong thẻ **Infrastructure**, cuộn xuống phần **Container instances**.
2. Hệ thống cần ghi nhận **2 máy chủ EC2** đang hoạt động với trạng thái *Active*. Đây là dấu hiệu cho thấy các máy chủ EC2 đã tự động gia nhập thành công vào cụm ECS thông qua kịch bản cấu hình User Data.

![Kiểm tra hạ tầng ECS Cluster](/images/5-Workshop/5.4/5.4.3/Screenshot%202026-09-26%20071649.png)

Hạ tầng cụm máy chủ Backend đã được chuẩn bị hoàn chỉnh. Trong phần tiếp theo, hệ thống sẽ được tích hợp với Load Balancer (Bộ cân bằng tải) đóng vai trò điều phối lưu lượng truy cập từ người dùng đi vào cụm máy chủ này.