---
title: "ECS Service Auto Scaling"
date: 2026-09-24
weight: 2
chapter: false
pre: " <b> 5.6.2. </b> "
---

# 5.6.2. Cấu hình ECS Service Auto Scaling theo tải

Email cảnh báo là tốt, nhưng nếu hệ thống có thể **tự động xử lý** tải cao thay vì gọi chúng ta dậy lúc nửa đêm thì tuyệt vời hơn. Chúng ta sẽ cấu hình để ECS tự động tăng số lượng Container lên khi CPU chạm mức 70%.

### Bước 1: Kích hoạt Auto Scaling trên ECS Service

1. Truy cập dịch vụ **ECS**, mở cụm `Eshop-ECS-Cluster`.
2. Tại tab **Services**, tích chọn `Eshop-Backend-Service` và nhấn nút **Update** (Cập nhật).
3. Kéo xuống phần **Service auto scaling** và tick vào ô **Use service auto scaling**.
4. Điền các tham số mở rộng:
   - **Minimum number of tasks** (Tối thiểu): `2` (Luôn duy trì ít nhất 2 container để dự phòng).
   - **Maximum number of tasks** (Tối đa): `10` (Tránh tạo quá nhiều container gây tốn kém).
5. Tại phần **Scaling policies**:
   - Policy type: Chọn **Target tracking** (Dò theo mục tiêu).
   - Policy name: `Scale-Out-High-CPU`.
   - ECS service metric: Chọn **ECSServiceAverageCPUUtilization**.
   - **Target value** (Mục tiêu): `70` (Khi CPU trung bình của cụm vượt 70%, ECS sẽ bật thêm Container mới để kéo số trung bình xuống; ngược lại sẽ tự động tắt bớt khi vắng khách).
   - Scale-out cooldown period: `60` (Chờ 60 giây giữa các lần tăng).
   - Scale-in cooldown period: `60` (Chờ 60 giây giữa các lần giảm).
6. Cuộn xuống cuối và nhấn **Update**.

![Cấu hình ECS Target Tracking Policy](/images/5-Workshop/5.6.2/ecs_target_tracking.png)

### Cơ chế hoạt động liên hoàn (Chain Reaction)

Lúc này, hệ thống của bạn đã là một cỗ máy tự động hoàn hảo:
1. Khách ồ ạt truy cập -> CPU của Container tăng lên 75%.
2. **ECS Service Auto Scaling** phát hiện vượt ngưỡng 70%, ra lệnh tạo thêm Container thứ 3.
3. Nếu 2 máy EC2 hiện tại đã cạn kiệt RAM/CPU không đủ chỗ nhét Container thứ 3, thì **ECS Capacity Provider** sẽ "cầu cứu".
4. **EC2 Auto Scaling Group** nhận được tín hiệu, lập tức bật thêm một máy chủ ảo EC2 mới.
5. Khi máy EC2 mới khởi động xong, Container thứ 3 được đưa vào chạy -> Tải giảm xuống mức an toàn.

Mọi thứ đều hoàn toàn tự động!