---
title: "Giám sát & Tự động mở rộng"
date: 2026-09-24
weight: 6
chapter: false
pre: " <b> 3.6. </b> "
---

# 3.6. Giám sát & Tự động mở rộng (Monitoring & Auto Scaling)

Trong quá trình vận hành thực tế, việc hoàn tất thiết lập hạ tầng mới chỉ là bước khởi đầu. Trong các sự kiện có lưu lượng truy cập đột biến (như Flash Sale), hệ thống E-shop phải có khả năng xử lý khối lượng kết nối khổng lồ. 

Chương này trình bày quy trình thiết lập **ECS Service Auto Scaling** nhằm cho phép hệ thống tự động nhân bản (scale out) thêm các Container Backend khi tải trọng CPU hoặc lưu lượng mạng tăng cao, đồng thời tự động thu hẹp quy mô (scale in) khi lưu lượng giảm để tối ưu hóa chi phí vận hành. Bên cạnh đó, **Amazon CloudWatch** được cấu hình để giám sát trạng thái sức khỏe toàn hệ thống và thiết lập các cảnh báo (Alarm) qua hệ thống email. Cuối cùng, **AWS CloudTrail** được kích hoạt để lưu trữ nhật ký kiểm tra (Audit log) đối với mọi thao tác thay đổi cấu hình hạ tầng, nhằm đảm bảo tuân thủ nghiêm ngặt các tiêu chuẩn bảo mật.

---

### Danh sách các nội dung triển khai chi tiết:

- **[3.6.1. Thiết lập CloudWatch Logs & Cảnh báo Alarms](3.6.1-cloudwatch-alarms/)**
- **[3.6.2. Cấu hình ECS Service Auto Scaling theo tải (Traffic/CPU)](3.6.2-service-autoscaling/)**
- **[3.6.3. Kích hoạt AWS CloudTrail để kiểm vết API](3.6.3-cloudtrail-audit/)**