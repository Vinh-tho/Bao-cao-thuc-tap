---
title: "Đóng gói & Đẩy Image lên ECR"
date: 2026-09-24
weight: 1
chapter: false
pre: " <b> 5.4.1. </b> "
---

# 5.4.1. Đóng gói Backend (Dockerfile) & Đẩy Image lên Amazon ECR

Để chạy ứng dụng Backend (API xử lý giỏ hàng, thanh toán...) trên cụm máy chủ ECS, chúng ta cần đóng gói mã nguồn thành một **Docker Image** và lưu trữ nó trên **Amazon ECR (Elastic Container Registry)** — một kho lưu trữ Image bảo mật của AWS.

*(Lưu ý: Để thực hành bài này, máy tính của bạn cần cài đặt sẵn **Docker Desktop** và cấu hình sẵn **AWS CLI**).*

### Bước 1: Tạo kho lưu trữ (Repository) trên Amazon ECR

1. Truy cập AWS Console, tìm kiếm **ECR** và chọn **Elastic Container Registry**.
2. Ở thanh menu bên trái, chọn **Repositories**, sau đó nhấn **Create repository**.
3. Tại mục **Visibility settings**, chọn **Private** (Chỉ cho phép hệ thống nội bộ kéo Image).
4. Tại **Repository name**, nhập `eshop-backend`.
5. Cuộn xuống cuối và nhấn **Create repository**.

![Tạo ECR Repository](/images/5-Workshop/5.4.1/create_ecr_repo.png)

### Bước 2: Xem các lệnh đẩy Image (Push Commands)

1. Trong danh sách Repositories, tick chọn `eshop-backend` vừa tạo.
2. Nhấn nút **View push commands** ở góc trên bên phải. AWS sẽ cung cấp sẵn 4 lệnh để bạn chạy trên Terminal/Command Prompt.

![View Push Commands](/images/5-Workshop/5.4.1/view_push_commands.png)

### Bước 3: Đóng gói và đẩy Image lên ECR

Mở Terminal (hoặc CMD/PowerShell) tại thư mục chứa mã nguồn Backend của dự án E-shop (nơi có chứa file `Dockerfile`). Chạy lần lượt 4 lệnh AWS đã cung cấp ở Bước 2:

1. **Xác thực Docker với ECR:**
   ```bash
   aws ecr get-login-password --region ap-southeast-1 | docker login --username AWS --password-stdin <AWS_ACCOUNT_ID>.dkr.ecr.ap-southeast-1.amazonaws.com
   ```

2. **Build Docker Image từ mã nguồn:**
   ```bash
   docker build -t eshop-backend .
   ```

3. **Gắn Tag cho Image vừa build:**
   ```bash
   docker tag eshop-backend:latest <AWS_ACCOUNT_ID>[.dkr.ecr.ap-southeast-1.amazonaws.com/eshop-backend:latest](https://.dkr.ecr.ap-southeast-1.amazonaws.com/eshop-backend:latest)
   ```

4. **Đẩy (Push) Image lên Amazon ECR:**
   ```bash
   docker push <AWS_ACCOUNT_ID>[.dkr.ecr.ap-southeast-1.amazonaws.com/eshop-backend:latest](https://.dkr.ecr.ap-southeast-1.amazonaws.com/eshop-backend:latest)
   ```

Sau khi lệnh Push chạy được 100%, hãy quay lại giao diện ECR trên AWS Console, nhấp vào tên repository `eshop-backend`. Bạn sẽ thấy Image có tag `latest` đã nằm gọn trong kho lưu trữ, sẵn sàng để hệ thống ECS kéo về và vận hành!