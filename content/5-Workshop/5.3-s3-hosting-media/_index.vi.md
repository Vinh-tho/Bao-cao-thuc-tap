---
title: "Triển khai Frontend & Lưu trữ S3"
date: 2026-09-24
weight: 3
chapter: false
pre: " <b> 5.3. </b> "
---

# 5.3. Triển khai Frontend & Lưu trữ Media với Amazon S3

Trong kiến trúc hiện đại, việc tách rời Frontend và Backend mang lại hiệu suất rất cao. Thay vì dùng máy chủ EC2 để trả về các file giao diện (HTML/CSS/JS) tĩnh, chúng ta sẽ tận dụng **Amazon S3 (Simple Storage Service)**. S3 không chỉ có chi phí cực rẻ mà còn có khả năng mở rộng vô hạn, chịu được lưu lượng truy cập khổng lồ mà không sợ sập máy chủ.

Trong chương này, chúng ta sẽ tạo 2 Bucket (Kho lưu trữ) trên S3: một cái đóng vai trò làm máy chủ Web tĩnh (Static Website Hosting) cho giao diện người dùng, và một cái để lưu trữ hình ảnh sản phẩm.

---

### Danh sách các bài thực hành chi tiết:

- **[5.3.1. Tạo S3 Bucket cho Frontend & Cấu hình Static Website Hosting](5.3.1-s3-frontend-hosting/)**
- **[5.3.2. Tạo S3 Bucket lưu trữ Media (Hình ảnh sản phẩm)](5.3.2-s3-media-storage/)**
- **[5.3.3. Tải mã nguồn Frontend và tài nguyên tĩnh lên S3](5.3.3-deploy-frontend/)**