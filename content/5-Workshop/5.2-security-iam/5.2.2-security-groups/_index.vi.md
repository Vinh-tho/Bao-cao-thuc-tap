---
title: "Khởi tạo Security Groups"
date: 2026-09-24
weight: 2
chapter: false
pre: " <b> 5.2.2. </b> "
---

# 5.2.2. Khởi tạo Security Groups cho ALB và ECS Backend

**Security Group (SG)** hoạt động như một tường lửa ảo ở cấp độ Instance để kiểm soát luồng giao thông mạng ra/vào. Để tuân thủ nguyên tắc bảo mật tối đa, chúng ta sẽ tạo 2 SG: 
1. **ALB Security Group**: Mở cửa cho Internet truy cập vào web.
2. **Backend Security Group**: Ẩn hoàn toàn khỏi Internet, chỉ cho phép nhận dữ liệu được chuyển tiếp từ ALB SG.

### Bước 1: Tạo Security Group cho Application Load Balancer (ALB)

1. Tại AWS Console, truy cập dịch vụ **VPC** (hoặc EC2), nhìn sang menu bên trái, cuộn xuống mục *Security* và chọn **Security Groups**.
2. Nhấn nút **Create security group**.
3. Điền thông tin cơ bản:
   - **Security group name**: `Eshop-ALB-SG`
   - **Description**: `Allow HTTP and HTTPS traffic from Internet to ALB`
   - **VPC**: Nhấn dấu `X` để xóa VPC mặc định, sau đó chọn `Eshop-VPC` của chúng ta.

![Cấu hình thông tin ALB Security Group](/images/5-Workshop/5.2.2/create_alb_sg_info.png)

4. Tại mục **Inbound rules**, nhấn **Add rule** 2 lần để thêm các cổng kết nối web:
   - Rule 1: Type = `HTTP`, Source = `Anywhere-IPv4` (`0.0.0.0/0`)
   - Rule 2: Type = `HTTPS`, Source = `Anywhere-IPv4` (`0.0.0.0/0`)
5. Giữ nguyên **Outbound rules** (cho phép All traffic) và nhấn nút **Create security group** ở cuối trang.

![Thêm Inbound rules cho ALB](/images/5-Workshop/5.2.2/create_alb_sg_rules.png)

### Bước 2: Tạo Security Group cho Backend (EC2 / ECS Task)

Bây giờ chúng ta tạo SG cho Backend. Điểm quan trọng nhất ở đây là Backend sẽ KHÔNG mở IP ra ngoài Internet (`0.0.0.0/0`), mà lấy chính cái SG của ALB làm nguồn (Source).

1. Quay lại danh sách Security Groups, nhấn **Create security group** một lần nữa.
2. Điền thông tin cơ bản:
   - **Security group name**: `Eshop-Backend-SG`
   - **Description**: `Allow traffic only from ALB`
   - **VPC**: Chọn `Eshop-VPC`.

![Cấu hình thông tin Backend Security Group](/images/5-Workshop/5.2.2/create_backend_sg_info.png)

3. Tại mục **Inbound rules**, nhấn **Add rule**:
   - Type = `All TCP` (để hỗ trợ Dynamic Port Mapping của ECS trên EC2).
   - Source = Chọn `Custom`, sau đó gõ chữ `sg-` vào ô tìm kiếm và chọn `Eshop-ALB-SG` từ danh sách xổ xuống.

![Thêm Inbound rule cho Backend từ ALB SG](/images/5-Workshop/5.2.2/create_backend_sg_rules.png)

4. Nhấn **Create security group**.

> **Lưu ý bảo mật:** Với thiết lập này, kể cả khi ai đó biết IP nội bộ của EC2/Container, họ cũng không thể truy cập trực tiếp. Yêu cầu bắt buộc phải đi qua cửa ngõ Load Balancer (ALB).