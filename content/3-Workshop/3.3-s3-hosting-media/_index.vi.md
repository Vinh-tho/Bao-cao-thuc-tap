---
title: "Triển khai Frontend & Lưu trữ S3"
date: 2026-09-24
weight: 3
chapter: false
pre: " <b> 3.3. </b> "
---

# 3.3. Triển khai Frontend & Lưu trữ Media với Amazon S3

Trong các kiến trúc hệ thống hiện đại, việc phân tách độc lập giữa Frontend và Backend mang lại hiệu suất vận hành tối ưu. Thay vì sử dụng máy chủ EC2 truyền thống để phân phối các tệp giao diện tĩnh (HTML/CSS/JS), kiến trúc này ứng dụng **Amazon S3 (Simple Storage Service)**. S3 là giải pháp lưu trữ tối ưu chi phí, cung cấp khả năng mở rộng linh hoạt và đảm bảo độ tin cậy cao ngay cả dưới tải lượng truy cập khổng lồ mà không gặp tình trạng quá tải máy chủ.

Chương này trình bày quy trình triển khai 2 Bucket (Kho lưu trữ) trên nền tảng S3: một Bucket đóng vai trò như máy chủ web tĩnh (Static Website Hosting) để phân phối giao diện người dùng, và một Bucket chuyên dụng cho việc lưu trữ hình ảnh sản phẩm.

---

### Danh sách các nội dung triển khai chi tiết:

- **[3.3.1. Khởi tạo S3 Bucket cho Frontend & Cấu hình Static Website Hosting](3.3.1-s3-frontend-hosting/)**
- **[3.3.2. Khởi tạo S3 Bucket lưu trữ Media (Hình ảnh sản phẩm)](3.3.2-s3-media-storage/)**
- **[3.3.3. Tải mã nguồn Frontend và tài nguyên tĩnh lên S3](3.3.3-deploy-frontend/)**