---
title: "Giám sát & Tự động mở rộng"
date: 2026-09-24
weight: 6
chapter: false
pre: " <b> 5.6. </b> "
---

# 5.6. Giám sát & Tự động mở rộng (Monitoring & Auto Scaling)

Trong vận hành thực tế, việc thiết lập xong hệ thống mới chỉ là bước khởi đầu. Khi sự kiện Flash Sale diễn ra, E-shop của bạn sẽ phải đối mặt với lượng truy cập khổng lồ. 

Trong chương này, chúng ta sẽ thiết lập **ECS Service Auto Scaling** để hệ thống tự động nhân bản thêm các Container Backend khi tải lượng CPU tăng cao, đồng thời thu nhỏ lại khi hết khách để tiết kiệm tiền. Ngoài ra, chúng ta sẽ dùng **Amazon CloudWatch** để theo dõi sức khỏe hệ thống, cài đặt cảnh báo (Alarm) qua email, và dùng **AWS CloudTrail** để ghi lại nhật ký (Audit) mọi thao tác thay đổi hạ tầng nhằm đảm bảo bảo mật.

---

### Danh sách các bài thực hành chi tiết:

- **[5.6.1. Thiết lập CloudWatch Logs & Cảnh báo Alarms](5.6.1-cloudwatch-alarms/)**
- **[5.6.2. Cấu hình ECS Service Auto Scaling theo tải (Traffic/CPU)](5.6.2-service-autoscaling/)**
- **[5.6.3. Kích hoạt AWS CloudTrail để kiểm vết API](5.6.3-cloudtrail-audit/)**