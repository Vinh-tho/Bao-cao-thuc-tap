---
title: "Khởi tạo VPC và Subnets"
weight: 1
pre: " <b> 3.1.1. </b> "
---

# 3.1.1. Khởi tạo VPC, Public Subnets và Private Subnets

Phần này trình bày quy trình khởi tạo Virtual Private Cloud (VPC) nhằm thiết lập không gian mạng riêng biệt cho hệ thống E-shop. Không gian mạng này sau đó sẽ được phân chia thành các Public Subnet (sử dụng để triển khai Load Balancer tiếp nhận lưu lượng từ Internet) và Private Subnet (đảm bảo an toàn cho các dịch vụ Backend, cách ly hoàn toàn khỏi Internet).

### Bước 1: Khởi tạo VPC

1. Truy cập dịch vụ **VPC** trên giao diện quản trị AWS Management Console.
2. Tại thanh điều hướng bên trái, chọn **Your VPCs** và nhấn nút **Create VPC**.
3. Cấu hình các thông số cơ bản cho VPC như sau:
   - **Resources to create**: Chọn `VPC only`.
   - **Name tag**: Nhập `Eshop-VPC` để định danh tài nguyên.
   - **IPv4 CIDR block**: Chọn `IPv4 CIDR manual input` và chỉ định dải mạng `10.0.0.0/16`.
4. Cuộn xuống cuối trang và nhấn **Create VPC** để hoàn tất.

![Khởi tạo VPC trên giao diện AWS](/images/3-Workshop/3.1/3.1.1/Screenshot%202026-09-25%20181154.png)
![Khởi tạo VPC trên giao diện AWS](/images/3-Workshop/3.1/3.1.1/Screenshot%202026-09-25%20181241.png)
![Khởi tạo VPC trên giao diện AWS](/images/3-Workshop/3.1/3.1.1/Screenshot%202026-09-25%20181332.png)

### Bước 2: Khởi tạo các Subnets (Mạng con)

Hệ thống yêu cầu thiết lập tổng cộng 4 Subnet (bao gồm 2 Public Subnet và 2 Private Subnet) phân bổ trên 2 Availability Zones (AZ) nhằm đảm bảo tính sẵn sàng cao (High Availability) cho kiến trúc mạng.

1. Tại thanh điều hướng của dịch vụ VPC, chọn **Subnets** và nhấn nút **Create subnet**.
2. Tại trường **VPC ID**, chọn VPC `Eshop-VPC` vừa khởi tạo.
3. Cấu hình thông số cho **Public Subnet 1**:
   - **Subnet name**: `Eshop-Public-Subnet-1`
   - **Availability Zone**: `ap-southeast-1a`
   - **IPv4 CIDR block**: `10.0.1.0/24`

![Tạo Public Subnet 1](/images/3-Workshop/3.1/3.1.1/Screenshot%202026-09-25%20182923.png)
![Tạo Public Subnet 1](/images/3-Workshop/3.1/3.1.1/Screenshot%202026-09-25%20183054.png)
![Tạo Public Subnet 1](/images/3-Workshop/3.1/3.1.1/Screenshot%202026-09-25%20183104.png)

4. Nhấn **Add new subnet** để tiếp tục cấu hình **Public Subnet 2** tại một AZ khác:
   - **Subnet name**: `Eshop-Public-Subnet-2`
   - **Availability Zone**: `ap-southeast-1b`
   - **IPv4 CIDR block**: `10.0.2.0/24`

![Tạo Public Subnet 2](/images/3-Workshop/3.1/3.1.1/Screenshot%202026-09-25%20183149.png)

5. Tiếp tục nhấn **Add new subnet** để cấu hình 2 Private Subnet tương tự:
   - **Private Subnet 1**: Subnet name = `Eshop-Private-Subnet-1`, Availability Zone = `ap-southeast-1a`, IPv4 CIDR block = `10.0.3.0/24`.
   - **Private Subnet 2**: Subnet name = `Eshop-Private-Subnet-2`, Availability Zone = `ap-southeast-1b`, IPv4 CIDR block = `10.0.4.0/24`.
6. Sau khi nhập đầy đủ thông tin cho 4 Subnet, cuộn xuống và nhấn **Create subnet** để thực thi.

![Tạo các Private Subnets](/images/3-Workshop/3.1/3.1.1/Screenshot%202026-09-25%20183252.png)
![Tạo các Private Subnets](/images/3-Workshop/3.1/3.1.1/Screenshot%202026-09-25%20183318.png)
![Tạo các Private Subnets](/images/3-Workshop/3.1/3.1.1/Screenshot%202026-09-25%20183405.png)

### Bước 3: Kích hoạt tự động cấp phát IPv4 công cộng cho Public Subnet

Để các tài nguyên được triển khai trong Public Subnet (ví dụ: Application Load Balancer) có khả năng tiếp nhận và giao tiếp với Internet, các Subnet này cần được cấu hình tự động gán địa chỉ Public IPv4.

1. Trong danh sách Subnet, đánh dấu chọn `Eshop-Public-Subnet-1`.
2. Tại góc trên bên phải, nhấn nút **Actions** và chọn **Edit subnet settings**.
3. Trong phần *Auto-assign IP settings*, đánh dấu vào tùy chọn **Enable auto-assign public IPv4 address**.
4. Nhấn **Save** để lưu lại cấu hình.

![Bật Auto-assign public IP](/images/3-Workshop/3.1/3.1.1/Screenshot%202026-09-25%20184711.png)
![Bật Auto-assign public IP](/images/3-Workshop/3.1/3.1.1/Screenshot%202026-09-25%20184734.png)
![Bật Auto-assign public IP](/images/3-Workshop/3.1/3.1.1/Screenshot%202026-09-25%20184829.png)

*(Lưu ý: Lặp lại thao tác tại Bước 3 cho `Eshop-Public-Subnet-2`)*.