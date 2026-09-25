---
title: "Khởi tạo VPC và Subnets"
weight: 1
pre: " <b> 5.1.1. </b> "
---

# 5.1.1. Khởi tạo VPC, Public Subnets và Private Subnets

Trong bài thực hành này, chúng ta sẽ tạo một Virtual Private Cloud (VPC) để làm không gian mạng riêng cho E-shop. Sau đó, chúng ta sẽ chia không gian này thành các Public Subnet (để đặt Load Balancer tiếp nhận lưu lượng từ Internet) và Private Subnet (để chạy Backend an toàn, ẩn khỏi Internet).

### Bước 1: Khởi tạo VPC

1. Truy cập dịch vụ **VPC** trên giao diện AWS Management Console.
2. Ở thanh công cụ bên trái, chọn **Your VPCs** và nhấn nút **Create VPC**.
3. Cấu hình các thông số cơ bản cho VPC:
   - **Resources to create**: Chọn `VPC only`.
   - **Name tag**: Nhập `Eshop-VPC` để dễ nhận diện.
   - **IPv4 CIDR block**: Chọn `IPv4 CIDR manual input` và nhập `10.0.0.0/16`.
4. Cuộn xuống cuối trang và nhấn **Create VPC**.

![Khởi tạo VPC trên giao diện AWS](/Bao-cao-thuc-tap/images/5-Workshop/5.1/5.1.1/Screenshot%202026-09-25%20181154.png)
![Khởi tạo VPC trên giao diện AWS](/Bao-cao-thuc-tap/images/5-Workshop/5.1/5.1.1/Screenshot%202026-09-25%20181241.png)
![Khởi tạo VPC trên giao diện AWS](/Bao-cao-thuc-tap/images/5-Workshop/5.1/5.1.1/Screenshot%202026-09-25%20181332.png)

### Bước 2: Tạo các Subnets (Mạng con)

Chúng ta sẽ tạo tổng cộng 4 Subnets (2 Public và 2 Private) trải dài trên 2 Availability Zones (AZs) để đảm bảo tính sẵn sàng cao (High Availability).

1. Ở menu bên trái của dịch vụ VPC, chọn **Subnets** và nhấn nút **Create subnet**.
2. Tại mục **VPC ID**, chọn `Eshop-VPC` mà chúng ta vừa tạo.
3. Điền thông tin cho **Public Subnet 1**:
   - **Subnet name**: `Eshop-Public-Subnet-1`
   - **Availability Zone**: `ap-southeast-1a`
   - **IPv4 CIDR block**: `10.0.1.0/24`

![Tạo Public Subnet 1](/Bao-cao-thuc-tap/images/5-Workshop/5.1/5.1.1/Screenshot%202026-09-25%20182923.png)
![Tạo Public Subnet 1](/Bao-cao-thuc-tap/images/5-Workshop/5.1/5.1.1/Screenshot%202026-09-25%20183054.png)
![Tạo Public Subnet 1](/Bao-cao-thuc-tap/images/5-Workshop/5.1/5.1.1/Screenshot%202026-09-25%20183104.png)

4. Nhấn vào nút **Add new subnet** để tiếp tục tạo **Public Subnet 2** ở một AZ khác:
   - **Subnet name**: `Eshop-Public-Subnet-2`
   - **Availability Zone**: `ap-southeast-1b`
   - **IPv4 CIDR block**: `10.0.2.0/24`

![Tạo Public Subnet 2](/Bao-cao-thuc-tap/images/5-Workshop/5.1/5.1.1/Screenshot%202026-09-25%20183149.png)

1. Tiếp tục nhấn **Add new subnet** để tạo 2 Private Subnets tương tự:
   - **Private Subnet 1**: Name = `Eshop-Private-Subnet-1`, AZ = `ap-southeast-1a`, CIDR = `10.0.3.0/24`.
   - **Private Subnet 2**: Name = `Eshop-Private-Subnet-2`, AZ = `ap-southeast-1b`, CIDR = `10.0.4.0/24`.
2. Sau khi điền đủ 4 Subnets, cuộn xuống và nhấn **Create subnet**.

![Tạo các Private Subnets](/Bao-cao-thuc-tap/images/5-Workshop/5.1/5.1.1/Screenshot%202026-09-25%20183252.png)
![Tạo các Private Subnets](/Bao-cao-thuc-tap/images/5-Workshop/5.1/5.1.1/Screenshot%202026-09-25%20183318.png)
![Tạo các Private Subnets](/Bao-cao-thuc-tap/images/5-Workshop/5.1/5.1.1/Screenshot%202026-09-25%20183405.png)

### Bước 3: Bật tự động cấp phát IP công cộng cho Public Subnet

Để các tài nguyên đặt trong Public Subnet (như Application Load Balancer sau này) có thể giao tiếp với bên ngoài, subnet đó cần tự động gán Public IPv4.

1. Trong danh sách Subnets, tick chọn `Eshop-Public-Subnet-1`.
2. Nhấn nút **Actions** ở góc trên bên phải, chọn **Edit subnet settings**.
3. Tại mục *Auto-assign IP settings*, tick chọn ô **Enable auto-assign public IPv4 address**.
4. Nhấn **Save**.

![Bật Auto-assign public IP](/Bao-cao-thuc-tap/images/5-Workshop/5.1/5.1.1/Screenshot%202026-09-25%20184711.png)
![Bật Auto-assign public IP](/Bao-cao-thuc-tap/images/5-Workshop/5.1/5.1.1/Screenshot%202026-09-25%20184734.png)
![Bật Auto-assign public IP](/Bao-cao-thuc-tap/images/5-Workshop/5.1/5.1.1/Screenshot%202026-09-25%20184829.png)

*(Lưu ý: Lặp lại Bước 3 cho cả `Eshop-Public-Subnet-2`)*.