---
title: "Cấu hình Route Tables"
weight: 3
pre: " <b> 3.1.3. </b> "
---

# 3.1.3. Cấu hình Route Tables cho luồng mạng

Phần này trình bày cấu hình Route Table (Bảng định tuyến) nhằm định hướng lưu lượng mạng trong VPC. Hệ thống yêu cầu triển khai 2 Route Table: một bảng dành cho **Public Subnets** (định tuyến lưu lượng ra Internet Gateway) và một bảng dành cho **Private Subnets** (định tuyến lưu lượng ra NAT Gateway).

### Bước 1: Khởi tạo và cấu hình Public Route Table

Route Table này cho phép các tài nguyên (ví dụ: Load Balancer) thiết lập kết nối trực tiếp với Internet.

1. Tại giao diện dịch vụ **VPC**, chọn **Route tables** ở thanh điều hướng bên trái.
2. Nhấn nút **Create route table**.
3. Cấu hình các thông số sau:
   - **Name**: `Eshop-Public-RT`
   - **VPC**: Chọn `Eshop-VPC`
4. Nhấn **Create route table** để thực thi.

![Tạo Public Route Table](/images/3-Workshop/3.1/3.1.3/Screenshot%202026-09-25%20191413.png)
![Tạo Public Route Table](/images/3-Workshop/3.1/3.1.3/Screenshot%202026-09-25%20191439.png)
![Tạo Public Route Table](/images/3-Workshop/3.1/3.1.3/Screenshot%202026-09-25%20191514.png)

5. Sau khi hoàn tất khởi tạo, tại màn hình chi tiết của `Eshop-Public-RT`, chọn thẻ **Routes** ở phía dưới và nhấn **Edit routes**.
6. Nhấn **Add route** và điền thông tin định tuyến:
   - **Destination**: Nhập `0.0.0.0/0` (Đại diện cho mọi dải địa chỉ Internet).
   - **Target**: Chọn **Internet Gateway**, sau đó chọn `Eshop-IGW` đã được khởi tạo ở phần trước.
7. Nhấn **Save changes** để lưu lại cấu hình.

![Thêm Route ra Internet Gateway](/images/3-Workshop/3.1/3.1.3/Screenshot%202026-09-25%20191714.png)
![Thêm Route ra Internet Gateway](/images/3-Workshop/3.1/3.1.3/Screenshot%202026-09-25%20191812.png)
![Thêm Route ra Internet Gateway](/images/3-Workshop/3.1/3.1.3/Screenshot%202026-09-25%20191828.png)

8. Chuyển sang thẻ **Subnet associations**, nhấn **Edit subnet associations**.
9. Đánh dấu chọn 2 Public Subnet (`Eshop-Public-Subnet-1` và `Eshop-Public-Subnet-2`), sau đó nhấn **Save associations** để hoàn tất liên kết.

![Liên kết Public Subnets](/images/3-Workshop/3.1/3.1.3/Screenshot%202026-09-25%20191854.png)
![Liên kết Public Subnets](/images/3-Workshop/3.1/3.1.3/Screenshot%202026-09-25%20191918.png)
![Liên kết Public Subnets](/images/3-Workshop/3.1/3.1.3/Screenshot%202026-09-25%20191936.png)

### Bước 2: Khởi tạo và cấu hình Private Route Table

Route Table này đảm bảo các dịch vụ Backend (EC2/ECS) được cách ly an toàn khỏi Internet, đồng thời cho phép định tuyến lưu lượng qua NAT Gateway để tải các bản cập nhật hoặc thư viện khi cần thiết.

1. Tương tự quy trình ở Bước 1, nhấn **Create route table**.
2. Cấu hình các thông số sau:
   - **Name**: `Eshop-Private-RT`
   - **VPC**: Chọn `Eshop-VPC`
3. Nhấn **Create route table** để thực thi.

![Tạo Private Route Table](/images/3-Workshop/3.1/3.1.3/Screenshot%202026-09-25%20192654.png)
![Tạo Private Route Table](/images/3-Workshop/3.1/3.1.3/Screenshot%202026-09-25%20192703.png)

4. Truy cập trang chi tiết của `Eshop-Private-RT`, chọn thẻ **Routes** và nhấn **Edit routes**.
5. Nhấn **Add route** và điền thông tin định tuyến:
   - **Destination**: Nhập `0.0.0.0/0`.
   - **Target**: Chọn **NAT Gateway**, sau đó chọn `Eshop-NAT-GW`.
6. Nhấn **Save changes** để lưu lại cấu hình.

![Thêm Route ra NAT Gateway](/images/3-Workshop/3.1/3.1.3/Screenshot%202026-09-25%20192718.png)
![Thêm Route ra NAT Gateway](/images/3-Workshop/3.1/3.1.3/Screenshot%202026-09-25%20192752.png)
![Thêm Route ra NAT Gateway](/images/3-Workshop/3.1/3.1.3/Screenshot%202026-09-25%20192810.png)

7. Chuyển sang thẻ **Subnet associations**, nhấn **Edit subnet associations**.
8. Đánh dấu chọn 2 Private Subnet (`Eshop-Private-Subnet-1` và `Eshop-Private-Subnet-2`), sau đó nhấn **Save associations** để hoàn tất liên kết.

![Liên kết Private Subnets](/images/3-Workshop/3.1/3.1.3/Screenshot%202026-09-25%20192837.png)
![Liên kết Private Subnets](/images/3-Workshop/3.1/3.1.3/Screenshot%202026-09-25%20192854.png)
![Liên kết Private Subnets](/images/3-Workshop/3.1/3.1.3/Screenshot%202026-09-25%20192909.png)

*(Lưu ý: Quá trình thiết lập hạ tầng mạng cơ bản bao gồm VPC, Subnets, Gateways và Route Tables đã hoàn tất. Kiến trúc này đảm bảo tính bảo mật và tuân thủ các tiêu chuẩn thực hành tốt nhất (Best Practices) của AWS cho hệ thống E-shop).*