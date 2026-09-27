---
title: "Xây dựng Backend Container"
date: 2026-09-24
weight: 4
chapter: false
pre: " <b> 3.4. </b> "
---

# 3.4. Xây dựng Backend Container với Docker, Amazon EC2 & ECS

Sau khi phân hệ Frontend đã được triển khai thành công trên S3, hệ thống yêu cầu một kiến trúc Backend mạnh mẽ nhằm xử lý các logic nghiệp vụ lõi như: quản lý giỏ hàng, xử lý thanh toán, tính toán khuyến mãi và truy xuất dữ liệu sản phẩm.

Thay vì thực thi mã nguồn trực tiếp trên các máy chủ ảo (EC2) theo phương pháp truyền thống, kiến trúc này sẽ tiến hành **Container hóa** Backend bằng Docker và triển khai lên nền tảng **Amazon ECS (Elastic Container Service)**. Phương pháp tiếp cận này hỗ trợ hệ thống E-shop dễ dàng cập nhật các phiên bản phần mềm mới mà không gặp rủi ro về xung đột môi trường (thiếu hụt thư viện, sai lệch phiên bản runtime), đồng thời cung cấp khả năng tự động mở rộng (Auto Scaling) linh hoạt nhằm đáp ứng các đợt gia tăng lưu lượng truy cập đột biến.

---

### Danh sách các nội dung triển khai chi tiết:

- **[3.4.1. Đóng gói Backend (Dockerfile) & Đẩy Image lên Amazon ECR](3.4.1-docker-ecr/)**
- **[3.4.2. Tạo EC2 Launch Template & EC2 Auto Scaling Group](3.4.2-ec2-asg/)**
- **[3.4.3. Khởi tạo ECS Cluster (EC2 Launch Type) & ECS Capacity Provider](3.4.3-ecs-cluster-cp/)**
- **[3.4.4. Cấu hình Application Load Balancer (ALB) & Target Group](3.4.4-alb-config/)**
- **[3.4.5. Định nghĩa ECS Task, Khởi tạo Service & Kết nối ALB](3.4.5-ecs-service/)**