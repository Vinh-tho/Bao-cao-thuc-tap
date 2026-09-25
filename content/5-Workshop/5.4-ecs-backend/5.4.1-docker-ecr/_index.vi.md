---
title: "Đóng gói & Đẩy Image lên ECR"
date: 2026-09-24
weight: 1
chapter: false
pre: " <b> 5.4.1. </b> "
---

# 5.4.1. Đóng gói Backend (Dockerfile) & Đẩy Image lên Amazon ECR

Để triển khai ứng dụng Backend (API xử lý nghiệp vụ) trên cụm máy chủ ECS, mã nguồn cần được đóng gói thành một Docker Image và lưu trữ tập trung trên Amazon ECR (Elastic Container Registry) — dịch vụ lưu trữ container image bảo mật của AWS.

_(Yêu cầu tiên quyết: Môi trường triển khai cục bộ cần được cài đặt sẵn Docker Desktop và cấu hình xác thực tài khoản AWS CLI)._

### Bước 1: Khởi tạo kho lưu trữ (Repository) trên Amazon ECR

1. Truy cập giao diện AWS Console, tìm kiếm và điều hướng đến dịch vụ **Elastic Container Registry (ECR)**.
2. Tại thanh điều hướng bên trái, dưới mục **Private registry**, chọn **Repositories**. Sau đó, nhấn nút **Create repository** màu cam ở góc trên bên phải màn hình.
3. Tại giao diện **Create private repository**, di chuyển đến phần **General settings** và nhập `eshop-backend` vào trường **Repository name**.
4. Giữ nguyên các cấu hình mặc định của hệ thống (bao gồm Image tag settings là _Mutable_ và Encryption settings là _AES-256_).
5. Cuộn xuống cuối trang và nhấn nút **Create** màu cam để hoàn tất quá trình khởi tạo.

![Tạo ECR Repository](/images/5-Workshop/5.4/5.4.1/Screenshot%202026-09-26%20020124.png)
![Tạo ECR Repository](/images/5-Workshop/5.4/5.4.1/Screenshot%202026-09-26%20020155.png)
![Tạo ECR Repository](/images/5-Workshop/5.4/5.4.1/Screenshot%202026-09-26%20020718.png)
![Tạo ECR Repository](/images/5-Workshop/5.4/5.4.1/Screenshot%202026-09-26%20020727.png)
![Tạo ECR Repository](/images/5-Workshop/5.4/5.4.1/Screenshot%202026-09-26%20020735.png)
![Tạo ECR Repository](/images/5-Workshop/5.4/5.4.1/Screenshot%202026-09-26%20020801.png)

### Bước 2: Truy xuất bộ lệnh triển khai (Push Commands)

1. Tại danh sách Repositories, chọn kho lưu trữ `eshop-backend` vừa khởi tạo.
2. Nhấn chọn **View push commands** ở góc trên cùng bên phải. Hệ thống AWS sẽ cung cấp chuỗi lệnh tiêu chuẩn để xác thực, biên dịch và đẩy Image.

![View Push Commands](/images/5-Workshop/5.4/5.4.1/Screenshot%202026-09-26%20021212.png)
![View Push Commands](/images/5-Workshop/5.4/5.4.1/Screenshot%202026-09-26%20021225.png)
![View Push Commands](/images/5-Workshop/5.4/5.4.1/Screenshot%202026-09-26%20021300.png)

### Bước 3: Đóng gói và đẩy Image lên ECR

Thông qua Terminal/Command Prompt (chạy tại thư mục gốc của dự án nơi chứa tệp `Dockerfile`), quá trình đóng gói và đẩy Image lên hệ thống được thực thi tuần tự qua các lệnh sau:

1. **Xác thực Docker client với Amazon ECR (sử dụng AWS CLI):**

   ```bash
   aws ecr get-login-password --region ap-southeast-1 | docker login --username AWS --password-stdin 753695657650.dkr.ecr.ap-southeast-1.amazonaws.com
   ```

2. **Biên dịch (Build) Docker Image từ mã nguồn:**

   ```bash
   docker build -t eshop-backend .
   ```

3. **Gắn thẻ (Tag) cho Image để chuẩn bị đẩy lên kho lưu trữ:**

   ```bash
   docker tag eshop-backend:latest 753695657650.dkr.ecr.ap-southeast-1.amazonaws.com/eshop-backend:latest
   ```

4. **Đẩy (Push) Image lên Amazon ECR:**
   ```bash
   docker push 753695657650.dkr.ecr.ap-southeast-1.amazonaws.com/eshop-backend:latest
   ```
![Đóng gói và đẩy Image lên ECR](/images/5-Workshop/5.4/5.4.1/Screenshot%202026-09-26%20024327.png)
![Đóng gói và đẩy Image lên ECR](/images/5-Workshop/5.4/5.4.1/Screenshot%202026-09-26%20025524.png)
![Đóng gói và đẩy Image lên ECR](/images/5-Workshop/5.4/5.4.1/Screenshot%202026-09-26%20025811.png)

Sau khi tiến trình tải lên hoàn tất 100%, kiểm tra lại trên giao diện ECR. Image với thẻ `latest` sẽ xuất hiện trong kho lưu trữ `eshop-backend`, xác nhận quá trình đóng gói và lưu trữ đã thành công.
