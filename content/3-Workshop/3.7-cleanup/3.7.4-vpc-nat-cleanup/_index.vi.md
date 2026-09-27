---
title: "Dọn dẹp VPC & NAT Gateway"
date: 2026-09-24
weight: 4
chapter: false
pre: " <b> 3.7.4. </b> "
---

# 3.7.4. Dọn dẹp VPC, NAT Gateway & Elastic IP (Tránh phí phát sinh)

Đây là bước đặc biệt quan trọng trong toàn bộ quy trình dọn dẹp, do **NAT Gateway và Elastic IP là các dịch vụ tính phí theo thời gian (theo giờ)**, ngay cả khi không có lưu lượng sử dụng. Quy trình kỹ thuật yêu cầu thực hiện xóa NAT Gateway trước, tiếp theo là giải phóng IP, và cuối cùng mới tiến hành xóa VPC.

### Bước 1: Xóa NAT Gateway

1. Truy cập dịch vụ **VPC** trên giao diện quản trị AWS Management Console.
2. Tại thanh điều hướng bên trái, chọn **NAT Gateways**.
3. Đánh dấu chọn NAT Gateway thuộc dự án E-shop (ví dụ: `Eshop-NAT-Gateway`).
4. Nhấn nút **Actions** -> **Delete NAT gateway**.
5. Nhập từ khóa `delete` vào ô xác nhận và nhấn **Delete** để thực thi.

> **Lưu ý quan trọng:** Trạng thái của NAT Gateway sẽ chuyển sang *Deleting*. **YÊU CẦU BẮT BUỘC** phải chờ khoảng 3 - 5 phút cho đến khi trạng thái chuyển hoàn toàn sang *Deleted* trước khi tiến hành Bước 2. Nếu thực hiện quá sớm, hệ thống AWS sẽ báo lỗi IP đang được sử dụng (in use).

### Bước 2: Giải phóng Elastic IP (EIP)

1. Tại giao diện VPC, cuộn thanh điều hướng bên trái xuống phần **Virtual private cloud** và chọn **Elastic IPs**.
2. Đánh dấu chọn địa chỉ IP tĩnh đã được cấp phát cho NAT Gateway trước đó.
3. Nhấn nút **Actions** -> **Release Elastic IP addresses**.
4. Xác nhận **Release**. (Thao tác này hoàn trả địa chỉ IP về lại vùng chứa tài nguyên của AWS, ngăn chặn việc phát sinh cước phí duy trì IP tĩnh).

### Bước 3: Xóa VPC (Dọn dẹp hàng loạt)

AWS hỗ trợ cơ chế dọn dẹp tự động: Khi thực hiện thao tác xóa VPC, hệ thống sẽ tự động gỡ bỏ toàn bộ các tài nguyên trực thuộc như Subnets, Route Tables, Internet Gateways và Security Groups.

1. Tại thanh điều hướng bên trái, chọn **Your VPCs**.
2. Đánh dấu chọn `Eshop-VPC`.
3. Nhấn nút **Actions** -> **Delete VPC**.
4. Bảng xác nhận sẽ liệt kê chi tiết toàn bộ các tài nguyên phụ thuộc chuẩn bị được gỡ bỏ. Kiểm tra lại danh sách, nhập từ khóa `delete` và nhấn nút **Delete**.

*(Lưu ý: Quy trình dọn dẹp tài nguyên đã hoàn tất toàn bộ. Môi trường tài khoản AWS đã trở lại trạng thái an toàn ban đầu, đảm bảo không phát sinh thêm bất kỳ khoản chi phí nào liên quan đến dự án triển khai này).*