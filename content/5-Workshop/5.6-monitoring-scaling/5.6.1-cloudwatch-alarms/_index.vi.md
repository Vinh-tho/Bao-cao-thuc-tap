---
title: "CloudWatch Logs & Alarms"
date: 2026-09-24
weight: 1
chapter: false
pre: " <b> 5.6.1. </b> "
---

# 5.6.1. Thiết lập CloudWatch Logs và cảnh báo Alarms

Mục này trình bày quy trình thiết lập cơ chế giám sát và cảnh báo tự động thông qua Amazon CloudWatch kết hợp Amazon SNS. Hệ thống được cấu hình nhằm mục đích tự động gửi thông báo qua email cho quản trị viên khi mức độ sử dụng CPU của dịch vụ Backend vượt ngưỡng an toàn, đảm bảo khả năng phản hồi và xử lý sự cố kịp thời.

### Bước 1: Khởi tạo kênh thông báo với Amazon SNS (Simple Notification Service)

Quy trình thiết lập kênh giao tiếp thông báo được thực hiện như sau:
1. Truy cập dịch vụ **SNS** trên giao diện AWS Console.
2. Tại thanh điều hướng bên trái, chọn **Topics** và nhấn **Create topic**.
3. **Type** (Loại): Lựa chọn **Standard**.
4. **Name** (Tên): Nhập `Eshop-Alert-Topic`, sau đó nhấn **Create topic**.
5. Tại giao diện chi tiết của Topic vừa khởi tạo, chuyển sang thẻ **Subscriptions** và chọn **Create subscription**.
6. **Protocol** (Giao thức): Lựa chọn **Email**.
7. **Endpoint**: Cung cấp địa chỉ email của quản trị viên nhận cảnh báo và nhấn **Create subscription**.
8. Truy cập hộp thư email đã đăng ký, mở thông báo xác nhận từ "AWS Notifications" và nhấn liên kết **Confirm subscription** để kích hoạt kênh thông báo.

![Xác nhận đăng ký nhận email cảnh báo từ SNS](/images/5-Workshop/5.6/5.6.1/Screenshot%202026-09-26%20230543.png)
![Xác nhận đăng ký nhận email cảnh báo từ SNS](/images/5-Workshop/5.6/5.6.1/Screenshot%202026-09-26%20230733.png)
![Xác nhận đăng ký nhận email cảnh báo từ SNS](/images/5-Workshop/5.6/5.6.1/Screenshot%202026-09-26%20231302.png)

### Bước 2: Thiết lập CloudWatch Alarm giám sát tài nguyên CPU

Quá trình cấu hình luồng giám sát tài nguyên thông qua CloudWatch được thực hiện như sau:
1. Truy cập dịch vụ **CloudWatch** trên giao diện AWS Console.
2. Tại thanh điều hướng bên trái, di chuyển đến **Alarms** > **All alarms** và nhấn **Create alarm**.
3. Nhấn **Select metric** để thiết lập thông số giám sát.
4. Điều hướng theo đường dẫn: `ECS` > `ClusterName, ServiceName`.
5. Tìm kiếm bản ghi có tên cụm (ClusterName) là `Eshop-ECS-Cluster` và dịch vụ (ServiceName) là `Eshop-Backend-Service`. Tích chọn thông số **CPUUtilization** và nhấn **Select metric**.
6. Tại khu vực cấu hình **Conditions** (Điều kiện):
   - **Threshold type** (Loại ngưỡng): Chọn **Static**.
   - **Whenever CPUUtilization is...**: Chọn **Greater/Equal (>=)**.
   - **Than...**: Nhập giá trị `80` (Cảnh báo được kích hoạt khi mức sử dụng CPU đạt từ 80% trở lên).
7. Nhấn **Next** để chuyển sang bước tiếp theo.
8. Tại khu vực **Notification** (Thông báo):
   - **Alarm state trigger**: Chọn **In alarm**.
   - Chọn **Select an existing SNS topic**.
   - Trong danh sách thả xuống, chỉ định topic `Eshop-Alert-Topic` đã khởi tạo tại Bước 1. Nhấn **Next**.
9. **Alarm name** (Tên cảnh báo): Nhập `Eshop-High-CPU-Alarm` và nhấn **Next**.
10. Tại trang Review, tiến hành rà soát thông số và nhấn **Create alarm** để hoàn tất khởi tạo.

![Tạo CloudWatch Alarm cho ECS CPU](/images/5-Workshop/5.6/5.6.1/Screenshot%202026-09-26%20231650.png)
![Tạo CloudWatch Alarm cho ECS CPU](/images/5-Workshop/5.6/5.6.1/Screenshot%202026-09-26%20231812.png)
![Tạo CloudWatch Alarm cho ECS CPU](/images/5-Workshop/5.6/5.6.1/Screenshot%202026-09-26%20231944.png)
![Tạo CloudWatch Alarm cho ECS CPU](/images/5-Workshop/5.6/5.6.1/Screenshot%202026-09-26%20232024.png)
![Tạo CloudWatch Alarm cho ECS CPU](/images/5-Workshop/5.6/5.6.1/Screenshot%202026-09-26%20232048.png)
![Tạo CloudWatch Alarm cho ECS CPU](/images/5-Workshop/5.6/5.6.1/Screenshot%202026-09-26%20232107.png)

**Kết luận:** Cấu hình tích hợp giám sát đã được thiết lập thành công. Khi phát sinh sự kiện tài nguyên CPU của dịch vụ Backend chạm mức ≥ 80%, CloudWatch sẽ tự động thay đổi trạng thái sang "In alarm" và điều hướng luồng thông báo qua SNS để gửi cảnh báo qua email, giúp tối ưu hóa công tác vận hành hệ thống.