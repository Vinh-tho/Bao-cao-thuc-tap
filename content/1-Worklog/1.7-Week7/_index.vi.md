---
title: "Worklog Tuần 7"
date: 2026-09-12
weight: 7
chapter: false
pre: " <b> 1.7. </b> "
---

### Mục tiêu tuần 7:

- Tìm hiểu Docker cơ bản và điều phối container với Amazon ECS.
- Viết Dockerfile cho backend .NET, build image và đẩy lên Amazon ECR.
- Tạo ECS Cluster (EC2 Launch Type), Capacity Provider, Task Definition và Service kết nối ALB.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| Thứ 7 – CN | Học Docker: image, container, Dockerfile, registry; thực hành docker build/run/tag/push. | 12/09/2026 | 13/09/2026 | <https://docs.docker.com> |
| Thứ 2 – Thứ 3 | Học điều phối container và các khái niệm ECS: cluster, task definition, service, EC2 vs Fargate, Capacity Provider, bridge network mode và dynamic port mapping. | 14/09/2026 | 15/09/2026 | <https://docs.aws.amazon.com/AmazonECS/latest/developerguide/> |
| Thứ 4 – Thứ 5 | Viết Dockerfile cho backend .NET, build image local, kiểm thử, tạo ECR repository eshop-backend, xác thực bằng AWS CLI và push image. | 16/09/2026 | 17/09/2026 | <https://cloudjourney.awsstudygroup.com/> |
| Thứ 6 | Tạo ECS cluster, Capacity Provider gắn với ASG, Task Definition (bridge mode, host port 0) và Service hai Task phía sau ALB; kiểm tra Task chuyển sang Running. | 18/09/2026 | 18/09/2026 | <https://cloudjourney.awsstudygroup.com/> |

### Chi tiết cấu hình kỹ thuật:

| Thành phần | Cấu hình / giá trị |
| --- | --- |
| ECR repository | eshop-backend, private, tag Mutable, mã hóa AES-256 |
| ECS cluster | Eshop-ECS-Cluster (tạo ở chế độ Fargate only trước để có service-linked role) |
| Capacity Provider | Eshop-ECS-CP, scaling type EC2 Auto Scaling, nhóm Eshop-ECS-ASG; trạng thái Active |
| Eshop-Backend-Task | Launch type EC2, network mode bridge, 0.5 vCPU, 0.5 GB, task role và execution role là Eshop-ECS-Task-Execution-Role |
| Container | eshop-backend-container, image = URI ECR thẻ :latest, container port 80, host port 0 (dynamic mapping), giao thức TCP |
| Eshop-Backend-Service | Capacity provider strategy Eshop-ECS-CP, desired tasks 2, load balancer Eshop-ALB, listener HTTP:80, target group Eshop-Backend-TG |

### Kiến thức đã học:

- Image và container: image là ứng dụng đã đóng gói; container là một bản đang chạy của nó.
- Task Definition là bản thiết kế (image, CPU/RAM, cổng, role); Service duy trì số task mong muốn và đăng ký chúng với load balancer.
- Bridge mode với host port 0 cho phép nhiều container dùng chung một host EC2 mà không đụng cổng, vì vậy backend SG mở All TCP từ ALB SG.
- Capacity Provider nối ECS cluster với ASG để ECS có thể yêu cầu thêm EC2 khi task không còn chỗ.
- Máy t2.micro có bộ nhớ hạn chế nên task được đặt ở 0.5 vCPU / 0.5 GB.

### Lệnh và mã đã sử dụng:

**Xác thực, build, tag và push image backend**

```bash
aws ecr get-login-password --region ap-southeast-1 | docker login --username AWS --password-stdin <ACCOUNT_ID>.dkr.ecr.ap-southeast-1.amazonaws.com
docker build -t eshop-backend .
docker tag eshop-backend:latest <ACCOUNT_ID>.dkr.ecr.ap-southeast-1.amazonaws.com/eshop-backend:latest
docker push <ACCOUNT_ID>.dkr.ecr.ap-southeast-1.amazonaws.com/eshop-backend:latest
```

### Khó khăn và cách giải quyết:

- Nền tảng Docker còn yếu nên ban đầu khó theo kịp khái niệm ECS. → Thực hành Docker trên máy trước khi làm trên ECS.

### Kết quả đạt được tuần 7:

- Hiểu cách Docker đóng gói ứng dụng và cách ECS duy trì đủ số container mong muốn.
- Build và đẩy image backend lên ECR.
- Hiểu vì sao bridge mode cần dynamic port mapping và Security Group phải cho phép ALB.
- Chuẩn bị quy trình container cho lần triển khai E-shop cuối cùng.

### Sản phẩm và minh chứng:

- Image gắn tag latest hiển thị trong ECR repository.
- Cluster hiển thị hai container instance active và các task ở trạng thái Running.

### Kế hoạch tuần tiếp theo:

- Tổng hợp toàn bộ kiến thức vào dự án E-shop cuối kỳ.
