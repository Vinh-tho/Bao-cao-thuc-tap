---
title: "Cấu hình Route Tables"
weight: 3
pre: " <b> 5.1.3. </b> "
---

# 5.1.3. Cấu hình Route Tables cho luồng mạng

**Route Table (Bảng định tuyến)** đóng vai trò như "biển chỉ dẫn" quyết định luồng giao thông mạng sẽ đi đâu. Chúng ta sẽ cần tạo 2 Route Tables: một cái dành cho **Public Subnets** (trỏ ra Internet Gateway) và một cái dành cho **Private Subnets** (trỏ ra NAT Gateway).

### Bước 1: Tạo và cấu hình Public Route Table

Route Table này sẽ cho phép các tài nguyên (như Load Balancer) kết nối trực tiếp với Internet.

1. Tại giao diện dịch vụ **VPC**, chọn **Route tables** ở menu bên trái.
2. Nhấn nút **Create route table**.
3. Điền thông tin:
   - **Name**: `Eshop-Public-RT`
   - **VPC**: Chọn `Eshop-VPC`
4. Nhấn **Create route table**.

![Tạo Public Route Table](/images/5-Workshop/5.1.3/create_public_rt.png)

5. Sau khi tạo xong, ở màn hình chi tiết của `Eshop-Public-RT`, chọn tab **Routes** ở nửa dưới màn hình và nhấn **Edit routes**.
6. Nhấn **Add route**:
   - **Destination**: Nhập `0.0.0.0/0` (Đại diện cho mọi địa chỉ Internet).
   - **Target**: Chọn **Internet Gateway**, sau đó chọn `Eshop-IGW` mà chúng ta đã tạo ở bài trước.
7. Nhấn **Save changes**.

![Thêm Route ra Internet Gateway](/images/5-Workshop/5.1.3/edit_public_routes.png)

8. Chuyển sang tab **Subnet associations**, nhấn **Edit subnet associations**.
9. Tick chọn 2 Public Subnets (`Eshop-Public-Subnet-1` và `Eshop-Public-Subnet-2`), sau đó nhấn **Save associations**.

![Liên kết Public Subnets](/images/5-Workshop/5.1.3/associate_public_subnets.png)

### Bước 2: Tạo và cấu hình Private Route Table

Route Table này đảm bảo Backend (EC2/ECS) không bị lộ ra Internet, nhưng vẫn có thể mượn đường qua NAT Gateway để tải các bản cập nhật cần thiết.

1. Tương tự Bước 1, nhấn **Create route table**.
2. Điền thông tin:
   - **Name**: `Eshop-Private-RT`
   - **VPC**: Chọn `Eshop-VPC`
3. Nhấn **Create route table**.

![Tạo Private Route Table](/images/5-Workshop/5.1.3/create_private_rt.png)

4. Mở chi tiết `Eshop-Private-RT`, chọn tab **Routes** và nhấn **Edit routes**.
5. Nhấn **Add route**:
   - **Destination**: Nhập `0.0.0.0/0`.
   - **Target**: Chọn **NAT Gateway**, sau đó chọn `Eshop-NAT-GW`.
6. Nhấn **Save changes**.

![Thêm Route ra NAT Gateway](/images/5-Workshop/5.1.3/edit_private_routes.png)

7. Chuyển sang tab **Subnet associations**, nhấn **Edit subnet associations**.
8. Tick chọn 2 Private Subnets (`Eshop-Private-Subnet-1` và `Eshop-Private-Subnet-2`), sau đó nhấn **Save associations**.

![Liên kết Private Subnets](/images/5-Workshop/5.1.3/associate_private_subnets.png)

*🎉 **Chúc mừng!** Bạn đã hoàn tất việc thiết lập toàn bộ hạ tầng mạng cơ bản (VPC, Subnets, Gateways, Route Tables) cực kỳ an toàn và chuẩn Best Practices của AWS cho dự án E-shop.*