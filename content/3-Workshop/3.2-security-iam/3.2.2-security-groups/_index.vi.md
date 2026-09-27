---
title: "Khởi tạo Security Groups"
date: 2026-09-24
weight: 2
chapter: false
pre: " <b> 3.2.2. </b> "
---

# 3.2.2. Khởi tạo Security Groups cho ALB và ECS Backend

**Security Group (SG)** hoạt động như một tường lửa ảo cấp độ Instance nhằm kiểm soát lưu lượng mạng vào/ra. Để tuân thủ nguyên tắc bảo mật, kiến trúc hệ thống yêu cầu thiết lập 2 SG: 
1. **ALB Security Group**: Cho phép lưu lượng từ Internet truy cập vào ứng dụng web.
2. **Backend Security Group**: Cách ly hoàn toàn khỏi Internet, chỉ cho phép tiếp nhận lưu lượng được chuyển tiếp từ ALB SG.

### Bước 1: Khởi tạo Security Group cho Application Load Balancer (ALB)

1. Tại giao diện AWS Management Console, truy cập dịch vụ **VPC** (hoặc EC2). Tại thanh điều hướng bên trái, cuộn xuống phần *Security* và chọn **Security Groups**.
2. Nhấn nút **Create security group**.
3. Cấu hình các thông tin cơ bản:
   - **Security group name**: `Eshop-ALB-SG`
   - **Description**: `Allow HTTP and HTTPS traffic from Internet to ALB`
   - **VPC**: Nhấn dấu `X` để xóa VPC mặc định, sau đó chọn `Eshop-VPC`.

![Cấu hình thông tin ALB Security Group](/images/3-Workshop/3.2/3.2.2/Screenshot%202026-09-25%20210249.png)
![Cấu hình thông tin ALB Security Group](/images/3-Workshop/3.2/3.2.2/Screenshot%202026-09-25%20210335.png)

4. Tại phần **Inbound rules**, nhấn **Add rule** 2 lần để cấu hình các cổng kết nối web:
   - Rule 1: Type = `HTTP`, Source = `Anywhere-IPv4` (`0.0.0.0/0`)
   - Rule 2: Type = `HTTPS`, Source = `Anywhere-IPv4` (`0.0.0.0/0`)
5. Giữ nguyên cấu hình **Outbound rules** (cho phép All traffic) và nhấn nút **Create security group** ở cuối trang để thực thi.

![Thêm Inbound rules cho ALB](/images/3-Workshop/3.2/3.2.2/Screenshot%202026-09-25%20210441.png)
![Thêm Inbound rules cho ALB](/images/3-Workshop/3.2/3.2.2/Screenshot%202026-09-25%20210507.png)
![Thêm Inbound rules cho ALB](/images/3-Workshop/3.2/3.2.2/Screenshot%202026-09-25%20210519.png)

### Bước 2: Khởi tạo Security Group cho Backend (EC2 / ECS Task)

Tiếp theo là quá trình khởi tạo SG cho Backend. Yêu cầu bảo mật quan trọng tại bước này là Backend KHÔNG mở kết nối ra Internet (`0.0.0.0/0`), mà chỉ chấp nhận lưu lượng có nguồn (Source) xuất phát từ SG của ALB.

1. Trở lại danh sách Security Groups và nhấn **Create security group**.
2. Cấu hình các thông tin cơ bản:
   - **Security group name**: `Eshop-Backend-SG`
   - **Description**: `Allow traffic only from ALB`
   - **VPC**: Chọn `Eshop-VPC`.

![Cấu hình thông tin Backend Security Group](/images/3-Workshop/3.2/3.2.2/Screenshot%202026-09-25%20211010.png)
![Cấu hình thông tin Backend Security Group](/images/3-Workshop/3.2/3.2.2/Screenshot%202026-09-25%20211035.png)

3. Tại phần **Inbound rules**, nhấn **Add rule**:
   - Type = `All TCP` (nhằm hỗ trợ Dynamic Port Mapping của ECS trên EC2).
   - Source = Chọn `Custom`, sau đó nhập `sg-` vào ô tìm kiếm và chọn `Eshop-ALB-SG` từ danh sách thả xuống.

![Thêm Inbound rule cho Backend từ ALB SG](/images/3-Workshop/3.2/3.2.2/Screenshot%202026-09-25%20211159.png)
![Thêm Inbound rule cho Backend từ ALB SG](/images/3-Workshop/3.2/3.2.2/Screenshot%202026-09-25%20211235.png)
![Thêm Inbound rule cho Backend từ ALB SG](/images/3-Workshop/3.2/3.2.2/Screenshot%202026-09-25%20211250.png)

4. Nhấn **Create security group** để hoàn tất.

> **Lưu ý bảo mật:** Với cấu hình này, các truy cập trực tiếp bằng IP nội bộ của EC2/Container sẽ bị chặn hoàn toàn. Mọi lưu lượng truy cập bắt buộc phải đi qua cổng Application Load Balancer (ALB).