---
title: "Worklog Tuần 6"
date: 2026-09-05
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
---

### Mục tiêu tuần 6:

- Tìm hiểu Elastic Load Balancing (Application Load Balancer) và EC2 Auto Scaling.
- Tạo Target Group, ALB, Launch Template và Auto Scaling Group.
- Kiểm chứng phân phối tải và khả năng tự co giãn.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| Thứ 7 – CN | Học ALB so với NLB, listener, Target Group, health check và các khái niệm Auto Scaling (Launch Template, ASG, desired/min/max, scaling policy). | 05/09/2026 | 06/09/2026 | <https://docs.aws.amazon.com/elasticloadbalancing/latest/application/> |
| Thứ 2 – Thứ 3 | Tạo Target Group (HTTP:80, health check tại /) và ALB Internet-facing trên hai Public Subnet với ALB Security Group riêng cho HTTP/HTTPS. | 07/09/2026 | 08/09/2026 | <https://cloudjourney.awsstudygroup.com/> |
| Thứ 4 – Thứ 5 | Tạo EC2 Launch Template (t2.micro, Security Group, IAM instance profile, User data) và Auto Scaling Group ở các Private Subnet (desired 2, min 1, max 4). | 09/09/2026 | 10/09/2026 | <https://docs.aws.amazon.com/autoscaling/ec2/userguide/> |
| Thứ 6 | Kiểm tra DNS của ALB, xem trạng thái target và quan sát ASG thay thế/thêm instance; dọn dẹp tài nguyên tính phí sau khi thực hành. | 11/09/2026 | 11/09/2026 | <https://cloudjourney.awsstudygroup.com/> |

### Chi tiết cấu hình kỹ thuật:

| Thành phần | Cấu hình / giá trị |
| --- | --- |
| Eshop-Backend-TG | Target type Instances, HTTP:80, health check HTTP path /, VPC Eshop-VPC; target do ECS đăng ký sau |
| Eshop-ALB | Internet-facing, IPv4, hai Public Subnet ở hai AZ, security group Eshop-ALB-SG, listener HTTP:80 → forward tới Eshop-Backend-TG |
| Eshop-ECS-Launch-Template | AMI Amazon ECS-optimized Amazon Linux, t2.micro, không key pair, security group Eshop-Backend-SG, IAM instance profile Eshop-EC2-Instance-Role, user data đăng ký instance vào ECS cluster |
| Eshop-ECS-ASG | Dùng launch template trên, VPC Eshop-VPC, Private Subnet 1 và 2, desired 2 / min 1 / max 4, không gắn load balancer (ALB tích hợp qua ECS) |

### Kiến thức đã học:

- Target type Instances được dùng vì bridge mode của ECS đăng ký EC2 cùng cổng host ngẫu nhiên.
- Health check quyết định ALB có gửi lưu lượng tới target hay không; target unhealthy bị đưa ra khỏi vòng phân phối.
- Launch Template lưu bản mẫu instance; Auto Scaling Group duy trì số instance mong muốn trên các AZ và thay thế instance lỗi.
- AMI ECS-optimized đã có sẵn Docker và ECS agent; user data cho agent biết cluster cần gia nhập.

### Lệnh và mã đã sử dụng:

**User data trong Launch Template**

```bash
#!/bin/bash
echo ECS_CLUSTER=Eshop-ECS-Cluster >> /etc/ecs/ecs.config
```

### Khó khăn và cách giải quyết:

- Target ở trạng thái unhealthy khi Security Group chưa cho phép ALB. → Chỉ cho phép lưu lượng từ ALB Security Group vào Backend Security Group.

### Kết quả đạt được tuần 6:

- Hiểu cách ALB, Target Group và health check phối hợp với nhau.
- Xây dựng ASG từ Launch Template chạy trong Private Subnet.
- Kiểm chứng phân phối lưu lượng và khả năng tự thay thế/co giãn instance.
- Các thành phần này được tái sử dụng cho ECS cluster ở tuần 7 và dự án cuối.

### Sản phẩm và minh chứng:

- Target Group và ALB ở trạng thái Active.
- ASG có hai instance chạy trong Private Subnet.

### Kế hoạch tuần tiếp theo:

- Học Docker và triển khai container trên ECS.
