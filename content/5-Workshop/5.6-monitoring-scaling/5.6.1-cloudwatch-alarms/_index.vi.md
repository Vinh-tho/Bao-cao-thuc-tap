---
title: "CloudWatch Logs & Alarms"
date: 2026-09-24
weight: 1
chapter: false
pre: " <b> 5.6.1. </b> "
---

# 5.6.1. Thiết lập CloudWatch Logs & Cảnh báo Alarms

**Amazon CloudWatch** là dịch vụ theo dõi và giám sát toàn diện của AWS. Chúng ta sẽ cài đặt một cảnh báo (Alarm) để hệ thống tự động gửi Email cho quản trị viên nếu CPU của Backend vượt ngưỡng an toàn.

### Bước 1: Tạo Topic gửi Email với SNS (Simple Notification Service)

Trước khi tạo cảnh báo, chúng ta cần một kênh giao tiếp để gửi email.
1. Truy cập dịch vụ **SNS** trên AWS Console.
2. Tại menu bên trái, chọn **Topics** và nhấn **Create topic**.
3. **Type**: Chọn **Standard**.
4. **Name**: `Eshop-Alert-Topic`. Nhấn **Create topic**.
5. Trong trang chi tiết của Topic vừa tạo, nhấn tab **Subscriptions** -> **Create subscription**.
6. **Protocol**: Chọn **Email**.
7. **Endpoint**: Nhập địa chỉ Email cá nhân của bạn. Nhấn **Create subscription**.
8. Mở hộp thư Email của bạn, tìm email từ "AWS Notifications" và nhấn link **Confirm subscription** để xác nhận.

![Xác nhận đăng ký nhận email cảnh báo từ SNS](/images/5-Workshop/5.6.1/sns_confirm_email.png)

### Bước 2: Tạo CloudWatch Alarm cho CPU

1. Truy cập dịch vụ **CloudWatch**.
2. Ở menu bên trái, chọn **All alarms** và nhấn **Create alarm**.
3. Nhấn **Select metric**.
4. Trình duyệt qua: `ECS` -> `ClusterName, ServiceName`.
5. Tìm dòng có tên Cluster là `Eshop-ECS-Cluster` và Service name là `Eshop-Backend-Service`, chọn metric **CPUUtilization** và nhấn **Select metric**.
6. Tại phần **Conditions**:
   - Threshold type: **Static**.
   - Whenever CPUUtilization is...: Chọn **Greater/Equal (>=)**.
   - Than...: Nhập `80` (Tức là cảnh báo khi CPU >= 80%).
7. Nhấn **Next**.
8. Tại phần **Notification**:
   - Chọn **In alarm**.
   - Chọn **Select an existing SNS topic**.
   - Chọn `Eshop-Alert-Topic` vừa tạo ở Bước 1. Nhấn **Next**.
9. **Alarm name**: Đặt tên là `Eshop-High-CPU-Alarm`. Nhấn **Next**.
10. Cuộn xuống cuối và nhấn **Create alarm**.

![Tạo CloudWatch Alarm cho ECS CPU](/images/5-Workshop/5.6.1/create_cloudwatch_alarm.png)

Từ giờ, nếu máy chủ Backend bị quá tải, bạn sẽ nhận được thông báo ngay lập tức qua Email!