---
title: "Xây dựng Nền tảng Mạng (Networking)"
date: 2026-09-24
weight: 1
chapter: false
pre: " <b> 5.1. </b> "
---

# 5.1. Xây dựng Nền tảng Mạng (Networking) với Amazon VPC

Trong chương này, chúng ta sẽ bắt đầu xây dựng nền móng hạ tầng mạng cho hệ thống Web E-shop. Việc thiết lập **Amazon VPC** theo chuẩn Best Practices với **Public Subnet** (dành cho Application Load Balancer) và **Private Subnet** (dành cho Backend EC2/ECS) là bước quan trọng nhất để đảm bảo tính bảo mật và tính sẵn sàng cao (High Availability) cho toàn bộ hệ thống.

### Yêu cầu tiên quyết:

1.  **Tài khoản AWS (AWS Account)** có quyền quản trị (Administrator Access) hoặc IAM User có đầy đủ quyền thao tác trên VPC, EC2, ECS, S3, Lambda, và IAM.
2.  **Trình duyệt web** (Google Chrome, Firefox, Safari hoặc Microsoft Edge).

---

### Bước 1: Đăng nhập Console và Chuyển Region

Để đảm bảo tính nhất quán trong suốt quá trình thực hành, chúng ta sẽ thống nhất sử dụng một Region gần với người dùng tại Việt Nam nhất.

1. Truy cập [AWS Management Console](https://console.aws.amazon.com/) và đăng nhập tài khoản của bạn.
2. Trên góc trên bên phải thanh điều hướng, chọn Region **Asia Pacific (Singapore) - ap-southeast-1**.

![Chuyển Region sang Singapore](/images/5-Workshop/5.1/chuyen%20vung.png)

---

### Tổng quan các phần thực hành tiếp theo:

Kiến trúc mạng của E-shop sẽ được triển khai bao gồm:

- **1 VPC** (Virtual Private Cloud) định tuyến không gian mạng riêng.
- **2 Public Subnets** ở 2 Availability Zones khác nhau (để kết nối Internet trực tiếp qua Internet Gateway, dùng cho ALB).
- **2 Private Subnets** ở 2 Availability Zones khác nhau (hoàn toàn ẩn khỏi Internet, có thể ra Internet một chiều thông qua NAT Gateway, dùng để chạy Container Backend).

Bạn hãy làm theo lần lượt các bài thực hành chi tiết dưới đây:

- [5.1.1. Khởi tạo VPC, Public Subnets và Private Subnets](5.1.1-create-vpc-subnets/)
- [5.1.2. Cấu hình Internet Gateway (IGW) và NAT Gateway](5.1.2-igw-nat-gateway/)
- [5.1.3. Cấu hình Route Tables cho luồng mạng](5.1.3-route-tables/)
