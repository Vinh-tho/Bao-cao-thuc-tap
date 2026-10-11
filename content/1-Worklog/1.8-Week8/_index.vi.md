---
title: "Worklog Tuần 8"
date: 2026-09-19
weight: 8
chapter: false
pre: " <b> 1.8. </b> "
---

### Mục tiêu tuần 8:

- Triển khai hoàn chỉnh E-shop trên AWS với kiến trúc ECS và Serverless.
- Kết hợp VPC, IAM, S3, ECR, EC2 Auto Scaling, ECS, ALB, RDS PostgreSQL, Lambda, CloudWatch, SNS và CloudTrail.
- Xử lý lỗi tích hợp, dọn dẹp tài nguyên và hoàn thiện báo cáo, workshop.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| Thứ 7 – CN | Chốt kiến trúc (frontend S3, backend ECS sau ALB trong Private Subnet, RDS PostgreSQL, Lambda xử lý ảnh, giám sát) và liệt kê dịch vụ, rủi ro chi phí, thứ tự triển khai. | 19/09/2026 | 20/09/2026 | <https://github.com/Vinh-tho/Eshop.git> |
| Thứ 2 – Thứ 5 | Chuẩn bị mã nguồn: clone repository, build Angular (eShop.Web), kiểm tra backend .NET với PostgreSQL, rà soát Dockerfile và lập kế hoạch các bước triển khai cho workshop. | 21/09/2026 | 24/09/2026 | <https://github.com/Vinh-tho/Eshop.git> |
| Thứ 6 | Buổi tối: dựng nền mạng (Eshop-VPC, 4 subnet, IGW, NAT Gateway, 2 Route Table); tạo IAM Role cho EC2, ECS Task Execution và Lambda; tạo Eshop-ALB-SG và Eshop-Backend-SG; tạo bucket S3 frontend với static hosting và Bucket Policy. | 25/09/2026 | 25/09/2026 | <https://cloudjourney.awsstudygroup.com/> |
| Thứ 7 | Rạng sáng: tạo bucket media (policy và CORS), upload bản build Angular lên bucket frontend và kiểm tra website endpoint; tạo ECR repository, build, tag và push image backend. | 26/09/2026 | 26/09/2026 | <https://cloudjourney.awsstudygroup.com/> |
| Thứ 7 | Buổi sáng: tạo Launch Template và ASG (desired 2), ECS cluster, Capacity Provider, Target Group, ALB, Task Definition và Service (2 Task); triển khai RDS PostgreSQL ở Private Subnet với Security Group cho DB chỉ cho phép cổng 5432 từ backend; xử lý lỗi triển khai (xem bên dưới) đến khi Task Running và Healthy. | 26/09/2026 | 26/09/2026 | <https://cloudjourney.awsstudygroup.com/> |
| Thứ 7 | Buổi tối: tạo hàm Lambda và S3 Event Notification, kiểm tra log trên CloudWatch; tạo SNS topic, alarm CPU và CloudTrail trail; cấu hình ECS Service Auto Scaling (target tracking CPU 70%, 2–10 task). | 26/09/2026 | 26/09/2026 | <https://cloudjourney.awsstudygroup.com/> |
| CN | Dọn dẹp theo thứ tự phụ thuộc (ALB/Target Group → ASG/Launch Template → ECS/ECR → Lambda/S3 → NAT Gateway/Elastic IP → VPC), hoàn thiện website Workshop, báo cáo và tự đánh giá. | 27/09/2026 | 27/09/2026 | <https://cloudjourney.awsstudygroup.com/> |

### Chi tiết cấu hình kỹ thuật:

| Thành phần | Cấu hình / giá trị |
| --- | --- |
| Bucket frontend | eshop-frontend-eshop-web; static hosting index.html / error.html; nội dung dist/e-shop.web/browser được upload vào gốc bucket |
| Bucket media | eshop-media-eshop-web; event notification → Eshop-Image-Resizer |
| Cơ sở dữ liệu | RDS PostgreSQL trong Private Subnet, security group SG-DB chỉ cho phép cổng 5432 từ security group của backend |
| ECS Service Auto Scaling | Tối thiểu 2, tối đa 10 task; target tracking theo ECSServiceAverageCPUUtilization = 70; cooldown scale-out và scale-in 60 giây; tên policy Scale-Out-High-CPU |
| Chuỗi mở rộng | CPU vượt 70% → ECS thêm task → EC2 hết chỗ → Capacity Provider báo cho ASG → EC2 mới gia nhập cluster → task được phân bổ |
| Liên kết demo | http://eshop-frontend-eshop-web.s3-website-ap-southeast-1.amazonaws.com |
| Mã nguồn | https://github.com/Vinh-tho/Eshop.git |
| Thứ tự dọn dẹp | ALB và Target Group → ASG và Launch Template → ECS service, cluster, task definition, ECR → Lambda, bucket S3, log group CloudWatch → NAT Gateway (chờ Deleted) → Elastic IP → VPC |

### Kiến thức đã học:

- Làm việc tích hợp nghĩa là gỡ lỗi xuyên qua nhiều dịch vụ: đọc thông báo lỗi, xác định tầng gây lỗi (mạng, quyền, tài nguyên), chỉ đổi một thứ mỗi lần và kiểm tra lại.
- Ước lượng tài nguyên rất quan trọng: SQL Server quá nặng với instance Free Tier, biểu hiện là task không bao giờ lên Healthy.
- Phòng thủ nhiều lớp: ALB ở public subnet, backend và database ở private subnet, Security Group nối chuỗi ALB → backend → database, IAM role thay cho key.
- Dọn dẹp theo thứ tự phụ thuộc; NAT Gateway phải ở trạng thái Deleted thì mới release được Elastic IP, và bucket phải rỗng trước khi xóa.

### Lệnh và mã đã sử dụng:

**Build frontend Angular**

```bash
cd eShop.Web
npm install
npm run build   # or: ng build
# output: dist/e-shop.web/browser
```

### Khó khăn và cách giải quyết:

- "Unable to assume the service linked role" khi tạo ECS cluster kèm ASG trên tài khoản mới. → Tạo cluster ở chế độ Fargate only trước để AWS sinh service-linked role, sau đó mới thêm Capacity Provider và ASG.
- ALB không đến được container do cổng động của bridge mode. → Host port 0, Target Group loại Instances, mở inbound All TCP trên Eshop-Backend-SG từ Eshop-ALB-SG.
- ECS Task không lên Running/Healthy khi dùng SQL Server (quá nặng với instance Free Tier). → Chuyển sang PostgreSQL nhẹ hơn, chỉnh lại cấu hình kết nối; Service sau đó qua health check của Target Group.
- Website S3 báo 403 hoặc không hiển thị. → Tắt Block Public Access, thêm policy GetObject có hậu tố /*, upload nội dung dist/e-shop.web/browser để index.html nằm ở gốc bucket.
- Không giải phóng được Elastic IP (in use) và không xóa được bucket. → Xóa NAT Gateway trước và chờ Deleted (3–5 phút) rồi mới release EIP; làm rỗng bucket (kể cả resized/) trước khi xóa.

### Kết quả đạt được tuần 8:

- Hoàn thành E-shop: frontend tĩnh trên S3, backend .NET container trên ECS (EC2 Launch Type) sau ALB trong Private Subnet, RDS PostgreSQL, Lambda hướng sự kiện, giám sát và kiểm vết.
- Áp dụng IAM Role đặc quyền tối thiểu và chuỗi Security Group (ALB SG → Backend SG → DB SG).
- Xuất bản Workshop 7 chương (3.1–3.7), liên kết demo, mã nguồn và báo cáo.
- Dọn dẹp toàn bộ tài nguyên tính phí để tránh chi phí ngoài ý muốn.

### Sản phẩm và minh chứng:

- Liên kết demo hoạt động (trước khi dọn dẹp) và ảnh chụp cho mọi chương 3.1–3.7.
- Website Workshop 7 chương, mã nguồn GitHub và báo cáo cuối kỳ.
- Hoàn thành tự đánh giá và phiếu phản hồi của sinh viên.

### Kế hoạch tuần tiếp theo:

- Học tiếp CI/CD và Infrastructure as Code, chuẩn bị chứng chỉ AWS Certified Cloud Practitioner.
