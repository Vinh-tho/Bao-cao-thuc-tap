---
title: "ECS Service Auto Scaling"
date: 2026-09-24
weight: 2
chapter: false
pre: " <b> 5.6.2. </b> "
---

# 5.6.2. Cấu hình ECS Service Auto Scaling theo tải

Mục này trình bày quy trình cấu hình tính năng Auto Scaling cho dịch vụ ECS nhằm mục đích tự động mở rộng và thu hẹp tài nguyên tính toán (số lượng Task/Container) dựa trên tải thực tế. Cấu hình này giúp hệ thống duy trì hiệu năng ổn định khi lưu lượng truy cập tăng đột biến, đồng thời tự động tối ưu hóa chi phí vận hành ở các thời điểm thấp điểm mà không cần sự can thiệp thủ công.

### Bước 1: Kích hoạt Auto Scaling trên ECS Service

1. Truy cập dịch vụ **ECS** trên giao diện AWS Console và mở cụm `Eshop-ECS-Cluster`.
2. Tại thẻ **Services**, chọn dịch vụ `Eshop-Backend-Service` và nhấn nút **Update** (Cập nhật).
3. Di chuyển đến khu vực **Service auto scaling** và tích chọn **Use service auto scaling**.
4. Cấu hình giới hạn số lượng thực thể (Task count):
   - **Minimum number of tasks** (Tối thiểu): `2` (Đảm bảo tính khả dụng cao - High Availability).
   - **Maximum number of tasks** (Tối đa): `10` (Kiểm soát giới hạn mở rộng để tối ưu ngân sách).
5. Tại phần **Scaling policies** (Chính sách mở rộng), thiết lập các tham số sau:
   - **Policy type**: Chọn **Target tracking** (Dò theo mục tiêu).
   - **Policy name**: Nhập `Scale-Out-High-CPU`.
   - **ECS service metric**: Chọn **ECSServiceAverageCPUUtilization**.
   - **Target value** (Giá trị mục tiêu): `70` (Hệ thống tự động Scale-out thêm Task khi CPU trung bình vượt 70% và Scale-in thu hồi Task khi tải giảm).
   - **Scale-out cooldown period**: `60` (Thời gian chờ giữa các chu kỳ tăng cường, tính bằng giây).
   - **Scale-in cooldown period**: `60` (Thời gian chờ giữa các chu kỳ thu hồi, tính bằng giây).
6. Kiểm tra lại thông số và nhấn **Update** ở cuối trang để áp dụng cấu hình.

![Cấu hình ECS Target Tracking Policy](/images/5-Workshop/5.6/5.6.2/Screenshot%202026-09-26%20232703.png)
![Cấu hình ECS Target Tracking Policy](/images/5-Workshop/5.6/5.6.2/Screenshot%202026-09-26%20232935.png)
![Cấu hình ECS Target Tracking Policy](/images/5-Workshop/5.6/5.6.2/Screenshot%202026-09-26%20232941.png)

### Đánh giá cơ chế tự động mở rộng liên hoàn (Chain-Reaction Scaling)

Sau khi hoàn tất cấu hình, kiến trúc Auto Scaling của hệ thống sẽ vận hành hoàn toàn tự động theo luồng sự kiện sau:
1. Lưu lượng truy cập hệ thống gia tăng dẫn đến mức sử dụng CPU của các Container hiện tại vượt ngưỡng (ví dụ: 75%).
2. **ECS Service Auto Scaling** phát hiện chỉ số giám sát vượt giá trị mục tiêu (70%), lập tức kích hoạt chính sách Scale-out và ra lệnh khởi tạo thêm Task (Container) thứ 3.
3. Trong trường hợp các máy chủ EC2 hiện hữu trong cụm không còn đủ tài nguyên (CPU/Memory) để cấp phát cho Task mới, **ECS Capacity Provider** sẽ ghi nhận trạng thái thiếu hụt tài nguyên (Capacity exhaustion).
4. Tín hiệu này kích hoạt **EC2 Auto Scaling Group (ASG)** tiến hành khởi chạy (provision) một máy chủ ảo EC2 mới.
5. Sau khi máy chủ EC2 mới hoàn tất khởi động và gia nhập cụm, Task thứ 3 sẽ được tự động phân bổ và triển khai lên máy chủ này, giúp giảm mức tải trung bình của toàn hệ thống về ngưỡng an toàn.