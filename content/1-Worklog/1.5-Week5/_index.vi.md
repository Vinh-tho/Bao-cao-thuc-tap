---
title: "Worklog Tuần 5"
date: 2026-08-29
weight: 5
chapter: false
pre: " <b> 1.5. </b> "
---

### Mục tiêu tuần 5:

- Hiểu vai trò của CloudWatch (giám sát) và CloudTrail (kiểm vết API).
- Thực hành Log Group, Metric, Dashboard và Alarm.
- Thiết lập cảnh báo qua email bằng SNS.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| Thứ 7 – CN | Học CloudWatch (log, metric, alarm, dashboard) và CloudTrail (management event, trail, event history); phân biệt giám sát với kiểm vết. | 29/08/2026 | 30/08/2026 | <https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/> |
| Thứ 2 – Thứ 3 | Tạo CloudWatch Dashboard cho EC2 và Lambda; xem Log Group của Lambda đã tạo ở tuần 4. | 31/08/2026 | 01/09/2026 | <https://cloudjourney.awsstudygroup.com/> |
| Thứ 4 – Thứ 5 | Tìm hiểu Metric và các trạng thái Alarm (OK / In alarm / Insufficient data), ngưỡng và chu kỳ đánh giá. | 02/09/2026 | 03/09/2026 | <https://cloudjourney.awsstudygroup.com/> |
| Thứ 6 | Tạo SNS topic Eshop-Alert-Topic với đăng ký email (xác nhận email); tạo alarm CPUUtilization ≥ 80% gửi thông báo về topic. Bật CloudTrail trail và xem Event history. | 04/09/2026 | 04/09/2026 | <https://cloudjourney.awsstudygroup.com/> |

### Chi tiết cấu hình kỹ thuật:

| Thành phần | Cấu hình / giá trị |
| --- | --- |
| SNS topic | Eshop-Alert-Topic, loại Standard |
| SNS subscription | Protocol Email, endpoint là email quản trị viên, trạng thái Confirmed sau khi bấm liên kết xác nhận |
| CloudWatch alarm | Eshop-High-CPU-Alarm; metric ECS → ClusterName, ServiceName → CPUUtilization (Eshop-ECS-Cluster / Eshop-Backend-Service); ngưỡng Static ≥ 80; khi In alarm → gửi tới Eshop-Alert-Topic |
| CloudTrail trail | Eshop-Audit-Trail (quick trail create) ghi vào bucket S3 tự tạo aws-cloudtrail-logs-… |
| Event history | Quan sát các sự kiện CreateTrail, CreateBucket, StartLogging với các trường Event name, Event time, User name, Event source, Resource name |

### Kiến thức đã học:

- Metric được xác định bằng namespace và dimension (với ECS: ClusterName và ServiceName); alarm so sánh metric với ngưỡng trong một khoảng thời gian.
- Các trạng thái alarm: OK, In alarm và Insufficient data; thông báo gắn với thay đổi trạng thái thông qua SNS.
- Đăng ký email của SNS ở trạng thái Pending cho đến khi được xác nhận.
- CloudWatch trả lời 'hệ thống đang thế nào' (log, metric); CloudTrail trả lời 'ai đã làm gì' (lời gọi API) và Event history lưu management event trong 90 ngày.
- Ngưỡng alarm (80%) dành cho thông báo cho người vận hành, còn mục tiêu Auto Scaling (70%) thêm ở tuần 8 hành động sớm hơn và tự động.

### Khó khăn và cách giải quyết:

- Cảnh báo không gửi về khi chưa xác nhận đăng ký SNS. → Biết rằng đăng ký email ở trạng thái Pending cho đến khi bấm liên kết xác nhận.

### Kết quả đạt được tuần 5:

- Phân biệt rõ CloudWatch (hệ thống đang làm gì) với CloudTrail (ai đã làm gì qua API).
- Tạo Dashboard, theo dõi log và metric.
- Cấu hình hoàn chỉnh Alarm kèm thông báo email qua SNS.
- Chuẩn bị các thành phần giám sát dùng lại trong dự án cuối (Eshop-High-CPU-Alarm, Eshop-Audit-Trail).

### Sản phẩm và minh chứng:

- Email cảnh báo nhận được từ AWS Notifications.
- CloudTrail Event history liệt kê các lời gọi API vừa thực hiện.

### Kế hoạch tuần tiếp theo:

- Học ALB và Auto Scaling với Launch Template, ASG.
