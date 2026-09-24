---
title: "Dọn dẹp VPC & NAT Gateway"
date: 2026-09-24
weight: 4
chapter: false
pre: " <b> 5.7.4. </b> "
---

# 5.7.4. Dọn dẹp VPC, NAT Gateway & Elastic IP (Tránh phí phát sinh)

Đây là bước quan trọng nhất trong quá trình dọn dẹp vì **NAT Gateway và Elastic IP là các dịch vụ tính phí theo giờ**, kể cả khi bạn không sử dụng. Bạn phải xóa NAT Gateway trước, sau đó giải phóng IP, và cuối cùng mới có thể xóa được VPC.

### Bước 1: Xóa NAT Gateway

1. Truy cập dịch vụ **VPC** trên AWS Console.
2. Tại menu bên trái, chọn **NAT Gateways**.
3. Tick chọn NAT Gateway của dự án E-shop (ví dụ: `Eshop-NAT-Gateway`).
4. Nhấn nút **Actions** -> **Delete NAT gateway**.
5. Gõ chữ `delete` vào ô xác nhận và nhấn **Delete**.

> **Lưu ý cực kỳ quan trọng:** Trạng thái của NAT Gateway sẽ chuyển sang *Deleting*. Bạn **BẮT BUỘC** phải đợi khoảng 3 - 5 phút cho đến khi trạng thái chuyển hẳn sang *Deleted* thì mới có thể thực hiện Bước 2. Nếu không, AWS sẽ báo lỗi IP đang được sử dụng.

![Xóa NAT Gateway](/images/5-Workshop/5.7.4/delete_nat_gateway.png)

### Bước 2: Giải phóng Elastic IP (EIP)

1. Vẫn ở giao diện VPC, cuộn menu bên trái xuống phần **Virtual private cloud** và chọn **Elastic IPs**.
2. Tick chọn địa chỉ IP tĩnh mà bạn đã cấp cho NAT Gateway.
3. Nhấn **Actions** -> **Release Elastic IP addresses**.
4. Xác nhận **Release**. (Việc này trả lại IP cho kho của AWS, giúp bạn không bị tính phí duy trì IP tĩnh).

![Giải phóng Elastic IP](/images/5-Workshop/5.7.4/release_elastic_ip.png)

### Bước 3: Xóa VPC (Dọn dẹp hàng loạt)

AWS có một tính năng rất thông minh: Khi bạn xóa VPC, nó sẽ tự động dọn dẹp toàn bộ các Subnet, Route Table, Internet Gateway và Security Group nằm bên trong VPC đó.

1. Tại menu bên trái, chọn **Your VPCs**.
2. Tick chọn `Eshop-VPC`.
3. Nhấn nút **Actions** -> **Delete VPC**.
4. Bảng xác nhận sẽ liệt kê toàn bộ các tài nguyên đi kèm sắp bị xóa theo. Hãy kiểm tra lại, gõ chữ `delete` và nhấn nút **Delete**.

![Xóa toàn bộ VPC và tài nguyên liên quan](/images/5-Workshop/5.7.4/delete_vpc.png)

Vậy là bạn đã hoàn tất quá trình dọn dẹp tài nguyên. Tài khoản AWS của bạn đã trở lại trạng thái an toàn, không còn phát sinh chi phí từ dự án này.