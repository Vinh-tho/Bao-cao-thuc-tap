---
title: "Xây dựng Nền tảng Mạng (Networking)"
date: 2026-09-24
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---

# 3.1. Xây dựng Nền tảng Mạng (Networking) với Amazon VPC

Chương này trình bày quy trình xây dựng nền móng hạ tầng mạng cho hệ thống Web E-shop. Việc thiết lập **Amazon VPC** theo chuẩn thực hành tốt nhất (Best Practices) với **Public Subnet** (dành cho Application Load Balancer) và **Private Subnet** (dành cho Backend EC2/ECS) là bước thiết yếu nhằm đảm bảo tính bảo mật và tính sẵn sàng cao (High Availability) cho toàn bộ hệ thống.

### Yêu cầu tiên quyết:

1. **Tài khoản AWS (AWS Account)** có quyền quản trị (Administrator Access) hoặc tài khoản IAM User được cấp đầy đủ quyền thao tác trên các dịch vụ VPC, EC2, ECS, S3, Lambda và IAM.
2. **Trình duyệt web** (Google Chrome, Firefox, Safari hoặc Microsoft Edge).

---

### Bước 1: Đăng nhập Console và chuyển Region

Để đảm bảo tính nhất quán trong toàn bộ quá trình triển khai, hệ thống sẽ được thiết lập tại một Region (Khu vực) có vị trí địa lý gần với người dùng tại Việt Nam nhất.

1. Truy cập [AWS Management Console](https://console.aws.amazon.com/) và đăng nhập vào hệ thống.
2. Tại góc trên bên phải thanh điều hướng, chọn Region **Asia Pacific (Singapore) - ap-southeast-1**.

![Chuyển Region sang Singapore](/images/3-Workshop/3.1/chuyen%20vung.png)

---

### Tổng quan kiến trúc mạng:

Kiến trúc mạng của hệ thống E-shop được thiết kế và triển khai với các thành phần bao gồm:

- **1 VPC** (Virtual Private Cloud) để định tuyến không gian mạng riêng.
- **2 Public Subnets** phân bổ ở 2 Availability Zones khác nhau (hỗ trợ kết nối Internet trực tiếp thông qua Internet Gateway, sử dụng cho ALB).
- **2 Private Subnets** phân bổ ở 2 Availability Zones khác nhau (cách ly hoàn toàn khỏi Internet, có khả năng kết nối ra Internet một chiều thông qua NAT Gateway, sử dụng để triển khai Container Backend).

Quy trình triển khai chi tiết được trình bày trong các phần sau:

- [3.1.1. Khởi tạo VPC, Public Subnets và Private Subnets](3.1.1-create-vpc-subnets/)
- [3.1.2. Cấu hình Internet Gateway (IGW) và NAT Gateway](3.1.2-igw-nat-gateway/)
- [3.1.3. Cấu hình Route Tables cho luồng mạng](3.1.3-route-tables/)