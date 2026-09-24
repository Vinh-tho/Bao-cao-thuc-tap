---
title: "Tải mã nguồn lên S3"
date: 2026-09-24
weight: 3
chapter: false
pre: " <b> 5.3.3. </b> "
---

# 5.3.3. Tải mã nguồn Frontend và tài nguyên tĩnh lên S3

Bây giờ các kho lưu trữ đã sẵn sàng, chúng ta sẽ tải mã nguồn giao diện web (HTML, CSS, JS) lên S3 Bucket dành cho Frontend để trang web chính thức hoạt động.

### Bước 1: Chuẩn bị mã nguồn Frontend

Trong thực tế, bạn sẽ có một thư mục build chứa mã nguồn. Nếu bạn đang thực hành theo Workshop này, hãy tải mã nguồn mẫu tại kho lưu trữ GitHub của dự án:

1. Truy cập link: `[https://github.com/YourOrganization/aws-eshop-workshop](https://github.com/YourOrganization/aws-eshop-workshop)` *(Link minh họa)*.
2. Tải mã nguồn về máy và giải nén. Bạn sẽ thấy một thư mục tên là `frontend-dist` chứa các file như `index.html`, `error.html`, `style.css` và thư mục `js/`.

### Bước 2: Tải file lên Frontend Bucket

1. Mở AWS Console, truy cập dịch vụ **S3** và click vào Bucket bạn đã tạo ở bài 5.3.1 (ví dụ: `eshop-frontend-nguyenvan-a`).
2. Ở tab **Objects**, nhấn nút **Upload**.
3. Nhấn nút **Add files** để tải lên các file riêng lẻ (`index.html`, `error.html`, `style.css`).
4. Nhấn nút **Add folder** để tải lên toàn bộ thư mục `js/` (và các thư mục khác nếu có).
5. Cuộn xuống dưới cùng và nhấn nút **Upload**. Chờ thanh tiến trình đạt 100%.
6. Sau khi hoàn tất, nhấn **Close** để quay lại danh sách Objects.

![Upload mã nguồn lên S3](/images/5-Workshop/5.3.3/upload_frontend_files.png)

### Bước 3: Truy cập Web E-shop của bạn

1. Tại trang chi tiết của Frontend Bucket, chuyển sang tab **Properties**.
2. Cuộn xuống dưới cùng tới mục **Static website hosting**.
3. Bạn sẽ thấy một đường link dưới dòng **Bucket website endpoint** (ví dụ: `[http://eshop-frontend-...s3-website-ap-southeast-1.amazonaws.com](http://eshop-frontend-...s3-website-ap-southeast-1.amazonaws.com)`).
4. Click vào đường link đó. Trình duyệt sẽ mở ra và... **Bùm!** Giao diện Web E-shop của bạn đã chính thức chạy trên Internet.

![Truy cập S3 Website Endpoint](/images/5-Workshop/5.3.3/visit_s3_endpoint.png)