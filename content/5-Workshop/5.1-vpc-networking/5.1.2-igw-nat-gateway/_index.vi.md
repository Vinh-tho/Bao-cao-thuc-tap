---
title: "Internet Gateway và NAT Gateway"
weight: 2
pre: " <b> 5.1.2. </b> "
---

# 5.1.2. Cấu hình Internet Gateway (IGW) và NAT Gateway

Để không gian mạng (VPC) của chúng ta có thể giao tiếp với Internet bên ngoài, chúng ta cần thiết lập **Internet Gateway (IGW)** (giúp Public Subnet kết nối hai chiều với Internet) và **NAT Gateway** (giúp các tài nguyên Container Backend trong Private Subnet truy cập Internet một chiều để tải các bản cập nhật/thư viện mà không bị lộ ra ngoài).

### Bước 1: Khởi tạo và gắn Internet Gateway (IGW)

1. Tại giao diện dịch vụ **VPC**, chọn **Internet gateways** ở menu bên trái.
2. Nhấn nút **Create internet gateway**.
3. Tại mục *Name tag*, nhập `Eshop-IGW` và nhấn nút **Create internet gateway**.

![Tạo Internet Gateway](/Bao-cao-thuc-tap/images/5-Workshop/5.1/5.1.2/Screenshot%202026-09-25%20185758.png)
![Tạo Internet Gateway](/Bao-cao-thuc-tap/images/5-Workshop/5.1/5.1.2/Screenshot%202026-09-25%20185838.png)
![Tạo Internet Gateway](/Bao-cao-thuc-tap/images/5-Workshop/5.1/5.1.2/Screenshot%202026-09-25%20185900.png)

4. Sau khi tạo xong, trạng thái của IGW sẽ là *Detached* (chưa gắn vào đâu). Nhấn nút **Actions** ở góc trên bên phải và chọn **Attach to VPC**.
5. Chọn `Eshop-VPC` (đã tạo ở bài 5.1.1) từ danh sách xổ xuống và nhấn **Attach internet gateway**.

![Gắn Internet Gateway vào VPC](/Bao-cao-thuc-tap/images/5-Workshop/5.1/5.1.2/Screenshot%202026-09-25%20185954.png)
![Gắn Internet Gateway vào VPC](/Bao-cao-thuc-tap/images/5-Workshop/5.1/5.1.2/Screenshot%202026-09-25%20190013.png)
![Gắn Internet Gateway vào VPC](/Bao-cao-thuc-tap/images/5-Workshop/5.1/5.1.2/Screenshot%202026-09-25%20190042.png)

### Bước 2: Khởi tạo NAT Gateway

NAT Gateway bắt buộc phải được đặt ở **Public Subnet** và cần một địa chỉ IP tĩnh (Elastic IP) để đại diện cho các máy chủ bên trong khi đi ra Internet.

1. Ở menu bên trái, chọn **NAT gateways** và nhấn nút **Create NAT gateway**.
2. Điền các thông tin sau:
   - **Name**: `Eshop-NAT-GW`
   - **Subnet**: Chọn `Eshop-Public-Subnet-1` (Bắt buộc phải chọn Public Subnet).
   - **Connectivity type**: Chọn `Public`.
3. Tại mục **Elastic IP allocation ID**, nhấn nút **Allocate Elastic IP** để AWS tự động cấp phát một địa chỉ IP công cộng tĩnh cho NAT Gateway này.
4. Kéo xuống dưới cùng và nhấn **Create NAT gateway**.

![Khởi tạo NAT Gateway](/Bao-cao-thuc-tap/images/5-Workshop/5.1/5.1.2/Screenshot%202026-09-25%20190518.png)
![Khởi tạo NAT Gateway](/Bao-cao-thuc-tap/images/5-Workshop/5.1/5.1.2/Screenshot%202026-09-25%20190720.png)
![Khởi tạo NAT Gateway](/Bao-cao-thuc-tap/images/5-Workshop/5.1/5.1.2/Screenshot%202026-09-25%20190803.png)

*(Lưu ý: Quá trình tạo NAT Gateway có thể mất vài phút. Trạng thái sẽ chuyển từ `Pending` sang `Available` khi quá trình hoàn tất. Bạn có thể sang bài tiếp theo trong lúc chờ đợi).*