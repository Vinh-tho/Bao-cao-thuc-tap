---
title: "Worklog Tuần 3"
date: 2026-08-15
weight: 3
chapter: false
pre: " <b> 1.3. </b> "
---

### Mục tiêu tuần 3:

- Tìm hiểu AWS Networking: VPC, Subnet, Internet Gateway, NAT Gateway và Route Table.
- Thiết kế và xây dựng VPC có Public/Private Subnet trên hai Availability Zone.
- Kiểm soát lưu lượng bằng Security Group và hiểu điểm khác với Network ACL.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| Thứ 7 – CN | Học CIDR, chia subnet, Public vs Private Subnet, Internet Gateway, NAT Gateway và Route Table. | 15/08/2026 | 16/08/2026 | <https://docs.aws.amazon.com/vpc/latest/userguide/> |
| Thứ 2 – Thứ 3 | Tạo VPC 10.0.0.0/16 và bốn subnet /24 (2 public, 2 private) tại ap-southeast-1a và ap-southeast-1b; bật auto-assign public IPv4 cho Public Subnet. | 17/08/2026 | 18/08/2026 | <https://cloudjourney.awsstudygroup.com/> |
| Thứ 4 | Tạo và gắn Internet Gateway; tạo NAT Gateway trong Public Subnet cùng Elastic IP. | 19/08/2026 | 19/08/2026 | <https://cloudjourney.awsstudygroup.com/> |
| Thứ 5 | Tạo hai Route Table: public (0.0.0.0/0 → IGW) và private (0.0.0.0/0 → NAT); liên kết đúng subnet. Học Security Group (stateful) và Network ACL (stateless). | 20/08/2026 | 20/08/2026 | <https://cloudjourney.awsstudygroup.com/> |
| Thứ 6 | Tạo EC2 trong VPC và kiểm tra đường truy cập Internet; ghi nhận NAT Gateway và Elastic IP tính phí theo giờ nên phải dọn dẹp sau khi thực hành. | 21/08/2026 | 21/08/2026 | <https://cloudjourney.awsstudygroup.com/> |

### Chi tiết cấu hình kỹ thuật:

| Thành phần | Cấu hình / giá trị |
| --- | --- |
| VPC | Eshop-VPC, IPv4 CIDR 10.0.0.0/16 |
| Eshop-Public-Subnet-1 | ap-southeast-1a, 10.0.1.0/24, bật auto-assign public IPv4 |
| Eshop-Public-Subnet-2 | ap-southeast-1b, 10.0.2.0/24, bật auto-assign public IPv4 |
| Eshop-Private-Subnet-1 | ap-southeast-1a, 10.0.3.0/24 |
| Eshop-Private-Subnet-2 | ap-southeast-1b, 10.0.4.0/24 |
| Internet Gateway | Eshop-IGW, gắn vào Eshop-VPC |
| NAT Gateway | Eshop-NAT-GW đặt tại Eshop-Public-Subnet-1, connectivity type Public, Elastic IP mới |
| Eshop-Public-RT | 0.0.0.0/0 → Eshop-IGW; liên kết với hai Public Subnet |
| Eshop-Private-RT | 0.0.0.0/0 → Eshop-NAT-GW; liên kết với hai Private Subnet |
| Eshop-ALB-SG | Inbound HTTP 80 và HTTPS 443 từ 0.0.0.0/0; outbound cho phép toàn bộ |
| Eshop-Backend-SG | Inbound All TCP chỉ từ Eshop-ALB-SG (hỗ trợ dynamic port mapping của ECS) |

### Kiến thức đã học:

- Mạng /16 có 65.536 địa chỉ và subnet /24 có 256 (AWS giữ lại 5 địa chỉ mỗi subnet), nên chia 10.0.0.0/16 thành bốn khối /24 vẫn còn rất nhiều chỗ để mở rộng.
- Một subnet là public chỉ vì route table của nó đưa 0.0.0.0/0 tới Internet Gateway; private subnet thì đưa tới NAT Gateway.
- NAT Gateway phải đặt trong public subnet và cần Elastic IP; nó chỉ cho phép kết nối đi ra từ tài nguyên private.
- Security Group có trạng thái (stateful) và gắn vào tài nguyên; Network ACL không có trạng thái (stateless) và gắn vào subnet. Dự án chỉ dùng Security Group.
- Dùng Security Group làm nguồn (Backend SG ← ALB SG) an toàn hơn mở dải IP: chỉ lưu lượng đi qua load balancer mới được chấp nhận.
- Hai AZ cho mỗi tầng giúp sẵn sàng cao; NAT Gateway và Elastic IP tính phí theo giờ.

### Khó khăn và cách giải quyết:

- Route Table và việc liên kết subnet khá trừu tượng, dễ nhầm. → Vẽ sơ đồ đường đi của lưu lượng (user → IGW → ALB; container → NAT → Internet) trước khi cấu hình.

### Kết quả đạt được tuần 3:

- Xây dựng được VPC gồm 2 Public Subnet, 2 Private Subnet, Internet Gateway, NAT Gateway và hai Route Table.
- Hiểu NAT Gateway cho phép đi ra một chiều còn Internet Gateway là hai chiều.
- Phân biệt được Security Group và Network ACL.
- Nhận diện các thành phần mạng tính phí theo giờ và sự cần thiết của quy trình dọn dẹp.

### Sản phẩm và minh chứng:

- Resource map của VPC gồm 4 subnet, IGW, NAT Gateway và 2 route table.
- Các route và liên kết subnet đã kiểm tra trên Console.
- Hai Security Group sẵn sàng cho ALB và backend.

### Kế hoạch tuần tiếp theo:

- Học AWS Lambda và xây dựng hàm xử lý ảnh kích hoạt từ S3.
