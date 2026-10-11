---
title: "Worklog Tuần 1"
date: 2026-08-01
weight: 1
chapter: false
pre: " <b> 1.1. </b> "
---

### Mục tiêu tuần 1:

- Nắm chương trình First Cloud AI Journey (FCAJ), lộ trình 8 tuần và quy định làm việc.
- Học nền tảng điện toán đám mây và hạ tầng toàn cầu của AWS (Region, Availability Zone, Edge).
- Tạo tài khoản AWS Free Tier để thực hành và làm quen với AWS Management Console.
- Phác thảo kiến trúc tổng quan đầu tiên cho dự án E-shop (Static Hosting, Container Backend, Event-Driven Serverless).

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| Thứ 7 – CN | Tham gia buổi định hướng: giới thiệu công ty, lộ trình 8 tuần, quy định đánh giá CLO (4 tuần/lần), làm quen với mentor. | 01/08/2026 | 02/08/2026 | <https://cloudjourney.awsstudygroup.com/> |
| Thứ 7 – CN | Học các khái niệm Cloud Computing: IaaS/PaaS/SaaS, mô hình trả tiền theo mức dùng, mô hình trách nhiệm chia sẻ. | 01/08/2026 | 02/08/2026 | <https://cloudjourney.awsstudygroup.com/> |
| Thứ 2 | Tạo và xác minh tài khoản AWS Free Tier, tìm hiểu giới hạn Free Tier và trang Billing để tránh phát sinh chi phí ngoài ý muốn. | 03/08/2026 | 03/08/2026 | <https://cloudjourney.awsstudygroup.com/> |
| Thứ 3 – Thứ 4 | Khám phá Management Console; tìm hiểu các nhóm dịch vụ chính (Compute, Storage, Networking, Database, Security) và cách chuyển Region (ap-southeast-1, Singapore). | 04/08/2026 | 05/08/2026 | <https://cloudjourney.awsstudygroup.com/> |
| Thứ 5 | Học hạ tầng toàn cầu (Region, AZ, Edge Location) và lý do hệ thống sẵn sàng cao cần tối thiểu hai AZ. | 06/08/2026 | 06/08/2026 | <https://cloudjourney.awsstudygroup.com/> |
| Thứ 6 | Đọc cấu trúc mã nguồn E-shop trên GitHub (Angular frontend, .NET backend, PostgreSQL) và vẽ sơ đồ kiến trúc nháp gồm ba phần: web tĩnh, backend container, xử lý ảnh theo sự kiện. | 07/08/2026 | 07/08/2026 | <https://github.com/Vinh-tho/Eshop.git> |

### Chi tiết cấu hình kỹ thuật:

| Thành phần | Cấu hình / giá trị |
| --- | --- |
| Region | ap-southeast-1 (Asia Pacific – Singapore), gần người dùng tại Việt Nam nhất |
| Loại tài khoản | Tài khoản AWS Free Tier dùng để thực hành |
| Frontend (dự kiến) | Build Angular → Amazon S3 Static Website Hosting |
| Backend (dự kiến) | Backend .NET đóng gói Docker → Amazon ECS (EC2 Launch Type) sau Application Load Balancer, đặt trong Private Subnet |
| Cơ sở dữ liệu (dự kiến) | PostgreSQL |
| Xử lý ảnh (dự kiến) | S3 Event → AWS Lambda (hướng sự kiện) |
| Vận hành (dự kiến) | CloudWatch + SNS (cảnh báo), CloudTrail (kiểm vết), IAM Role (đặc quyền tối thiểu) |

### Kiến thức đã học:

- Các mô hình dịch vụ cloud: IaaS (ví dụ EC2), PaaS, SaaS; trong E-shop, S3 là lưu trữ, ECS/EC2 là tính toán container và Lambda là function-as-a-service.
- Mô hình trách nhiệm chia sẻ: AWS bảo mật hạ tầng đám mây (data center, phần cứng); khách hàng bảo mật những gì chạy trên đó (IAM, Security Group, dữ liệu, ứng dụng).
- Region và Availability Zone: Region là khu vực địa lý, AZ là data center độc lập bên trong; dùng hai AZ giúp hệ thống chịu được sự cố của một AZ.
- Mô hình tính phí theo mức dùng: một số dịch vụ tính phí theo thời gian dù không có tải (NAT Gateway, Elastic IP, Application Load Balancer), nên mỗi bài lab phải kết thúc bằng dọn dẹp.

### Khó khăn và cách giải quyết:

- Nhiều thuật ngữ mới xuất hiện cùng lúc (Region, AZ, IaaS, shared responsibility). → Lập bảng thuật ngữ cá nhân và tự diễn giải lại từng khái niệm.

### Kết quả đạt được tuần 1:

- Hiểu các thành phần chính của hạ tầng toàn cầu AWS và cách thao tác trên Console.
- Tạo và xác minh thành công tài khoản AWS Free Tier.
- Phân biệt được dịch vụ nằm trong và ngoài Free Tier, hình thành thói quen kiểm soát chi phí cho các tuần sau.
- Hoàn thành bản nháp kiến trúc đầu tiên cho dự án E-shop.

### Sản phẩm và minh chứng:

- Bảng thuật ngữ cloud cá nhân.
- Bản phác thảo kiến trúc đầu tiên của E-shop (ba phần: web tĩnh, backend container, serverless hướng sự kiện).
- Tài khoản AWS đã xác minh, sẵn sàng thực hành.

### Kế hoạch tuần tiếp theo:

- Học EC2, S3, IAM và thực hành trên tài nguyên thật.
