---
title: "Internet Gateway và NAT Gateway"
weight: 2
pre: " <b> 3.1.2. </b> "
---

# 3.1.2. Cấu hình Internet Gateway (IGW) và NAT Gateway

Phần này trình bày quy trình cấu hình Internet Gateway (IGW) và NAT Gateway để thiết lập kết nối Internet cho Virtual Private Cloud (VPC). Internet Gateway đóng vai trò thiết lập kết nối hai chiều giữa Public Subnet và Internet. Trong khi đó, NAT Gateway cho phép các tài nguyên (như Container Backend) nằm trong Private Subnet truy cập Internet một chiều (nhằm mục đích tải các bản cập nhật hoặc thư viện) mà không bị lộ địa chỉ IP ra mạng công cộng.

### Bước 1: Khởi tạo và gắn Internet Gateway (IGW)

1. Tại giao diện dịch vụ **VPC**, chọn **Internet gateways** ở thanh điều hướng bên trái.
2. Nhấn nút **Create internet gateway**.
3. Tại trường *Name tag*, nhập `Eshop-IGW` để định danh và nhấn nút **Create internet gateway**.

![Tạo Internet Gateway](/images/3-Workshop/3.1/3.1.2/Screenshot%202026-09-25%20185758.png)
![Tạo Internet Gateway](/images/3-Workshop/3.1/3.1.2/Screenshot%202026-09-25%20185838.png)
![Tạo Internet Gateway](/images/3-Workshop/3.1/3.1.2/Screenshot%202026-09-25%20185900.png)

4. Sau khi khởi tạo, IGW sẽ ở trạng thái *Detached* (chưa được gắn vào VPC). Nhấn nút **Actions** ở góc trên bên phải và chọn **Attach to VPC**.
5. Chọn `Eshop-VPC` (đã khởi tạo ở phần 3.1.1) từ danh sách thả xuống và nhấn **Attach internet gateway**.

![Gắn Internet Gateway vào VPC](/images/3-Workshop/3.1/3.1.2/Screenshot%202026-09-25%20185954.png)
![Gắn Internet Gateway vào VPC](/images/3-Workshop/3.1/3.1.2/Screenshot%202026-09-25%20190013.png)
![Gắn Internet Gateway vào VPC](/images/3-Workshop/3.1/3.1.2/Screenshot%202026-09-25%20190042.png)

### Bước 2: Khởi tạo NAT Gateway

NAT Gateway yêu cầu phải được triển khai tại **Public Subnet** và cần được gán một địa chỉ IP tĩnh (Elastic IP) nhằm đại diện cho các tài nguyên nội bộ khi định tuyến lưu lượng ra Internet.

1. Tại thanh điều hướng bên trái, chọn **NAT gateways** và nhấn nút **Create NAT gateway**.
2. Cấu hình các thông số sau:
   - **Name**: `Eshop-NAT-GW`
   - **Subnet**: Chọn `Eshop-Public-Subnet-1` (Yêu cầu bắt buộc là Public Subnet).
   - **Connectivity type**: Chọn `Public`.
3. Tại phần **Elastic IP allocation ID**, nhấn nút **Allocate Elastic IP** để hệ thống AWS tự động cấp phát một địa chỉ IP công cộng tĩnh cho NAT Gateway.
4. Cuộn xuống cuối trang và nhấn **Create NAT gateway** để thực thi.

![Khởi tạo NAT Gateway](/images/3-Workshop/3.1/3.1.2/Screenshot%202026-09-25%20190518.png)
![Khởi tạo NAT Gateway](/images/3-Workshop/3.1/3.1.2/Screenshot%202026-09-25%20190720.png)
![Khởi tạo NAT Gateway](/images/3-Workshop/3.1/3.1.2/Screenshot%202026-09-25%20190803.png)

*(Lưu ý: Quá trình khởi tạo NAT Gateway có thể mất vài phút. Trạng thái hệ thống sẽ chuyển từ `Pending` sang `Available` khi quá trình hoàn tất. Quản trị viên có thể tiếp tục cấu hình các phần khác trong thời gian chờ).*