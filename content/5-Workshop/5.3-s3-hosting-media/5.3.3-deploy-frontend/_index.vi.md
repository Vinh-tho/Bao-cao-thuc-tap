---
title: "Tải mã nguồn lên S3"
date: 2026-09-24
weight: 3
chapter: false
pre: " <b> 5.3.3. </b> "
---

# 5.3.3. Tải mã nguồn Frontend và tài nguyên tĩnh lên S3

Sau khi hoàn tất cấu hình các S3 Bucket, bước tiếp theo là tải mã nguồn giao diện web (đã được biên dịch sang định dạng HTML, CSS, JS tĩnh) lên Bucket Frontend để triển khai ứng dụng.

### Bước 1: Chuẩn bị và biên dịch (Build) mã nguồn Frontend

Mã nguồn tổng thể của dự án được lưu trữ và quản lý tập trung trên nền tảng GitHub tại địa chỉ:  
`https://github.com/Vinh-tho/Eshop.git`

Quá trình triển khai bắt đầu từ việc tải (clone) mã nguồn về môi trường cục bộ. Sau khi di chuyển vào thư mục chứa mã nguồn Frontend (`eShop.Web`), tiến hành thực thi các lệnh cài đặt và biên dịch (`npm install` và `npm run build` hoặc `ng build`). 
Hệ thống sẽ tự động tối ưu, đóng gói toàn bộ dự án và xuất ra các tài nguyên tĩnh tại thư mục `dist`. Thư mục này bao gồm các tệp tin cấu trúc và giao diện cần thiết như `index.html`, các tệp `.js`, `.css` và thư mục `assets`.

![Chuẩn bị và biên dịch (Build) mã nguồn Frontend](/images/5-Workshop/5.3/5.3.3/Screenshot%202026-09-26%20011303.png)

### Bước 2: Tải tài nguyên lên S3 Bucket

1. Truy cập dịch vụ **S3** trên giao diện quản trị AWS Console và mở Bucket đã được khởi tạo cho Frontend (cụ thể là `eshop-frontend-eshop-web`).
2. Tại tab **Objects**, chọn nút **Upload** để chuyển sang giao diện tải lên dữ liệu.
3. Tại màn hình Upload, tiến hành đưa các tài nguyên tĩnh (vừa được biên dịch ở Bước 1) lên hệ thống. Người triển khai có thể sử dụng một trong hai phương pháp:
   * **Phương pháp 1 (Kéo thả - Khuyến nghị):** Mở thư mục chứa mã nguồn biên dịch trên máy tính (đường dẫn cụ thể là `dist/e-shop.web/browser`). Bôi đen toàn bộ các tệp tin (bao gồm `index.html`, các tệp `.js`, `.css`...) và thư mục con, sau đó kéo và thả trực tiếp vào vùng *"Drag and drop files and folders..."* trên giao diện AWS.
   * **Phương pháp 2 (Sử dụng nút chức năng):** Nhấn nút **Add files** để chọn và tải lên toàn bộ các tệp tin lẻ (`index.html`, các tệp `.js`, `.css`). Trường hợp có các thư mục con (ví dụ: `assets`), tiếp tục nhấn nút **Add folder** để tải lên.
4. **Yêu cầu kỹ thuật:** Cần đảm bảo tệp `index.html` được tải lên nằm ngay tại vị trí thư mục gốc của Bucket (không bị bọc bên trong một thư mục khác) để tính năng Static Website Hosting có thể nhận diện và khởi chạy chính xác.
5. Cuộn xuống cuối trang, nhấn nút **Upload** màu cam để bắt đầu tiến trình. Chờ đợi tiến trình hoàn tất 100%, sau đó nhấn **Close** để kiểm tra lại danh sách các đối tượng (Objects) đã xuất hiện trong Bucket.

![Upload mã nguồn Frontend lên S3 Bucket](/images/5-Workshop/5.3/5.3.3/Screenshot%202026-09-26%20011816.png)
![Upload mã nguồn Frontend lên S3 Bucket](/images/5-Workshop/5.3/5.3.3/Screenshot%202026-09-26%20011824.png)
![Upload mã nguồn Frontend lên S3 Bucket](/images/5-Workshop/5.3/5.3.3/Screenshot%202026-09-26%20012427.png)
![Upload mã nguồn Frontend lên S3 Bucket](/images/5-Workshop/5.3/5.3.3/Screenshot%202026-09-26%20012508.png)
![Upload mã nguồn Frontend lên S3 Bucket](/images/5-Workshop/5.3/5.3.3/Screenshot%202026-09-26%20012549.png)
![Upload mã nguồn Frontend lên S3 Bucket](/images/5-Workshop/5.3/5.3.3/Screenshot%202026-09-26%20012620.png)
![Upload mã nguồn Frontend lên S3 Bucket](/images/5-Workshop/5.3/5.3.3/Screenshot%202026-09-26%20012637.png)
![Upload mã nguồn Frontend lên S3 Bucket](/images/5-Workshop/5.3/5.3.3/Screenshot%202026-09-26%20012658.png)

### Bước 3: Kiểm tra hoạt động của website

1. Tại trang quản lý chi tiết của Bucket `eshop-frontend-eshop-web`, chuyển sang tab **Properties**.
2. Di chuyển đến mục **Static website hosting** và truy cập vào đường dẫn được AWS cung cấp tại phần **Bucket website endpoint** (ví dụ: `http://eshop-frontend-eshop-web.s3-website-ap-southeast-1.amazonaws.com`).
3. Trình duyệt sẽ điều hướng đến địa chỉ trên và hiển thị giao diện của hệ thống E-shop, xác nhận quá trình triển khai Frontend lên dịch vụ S3 đã thành công.

![Truy cập S3 Website Endpoint](/images/5-Workshop/5.3/5.3.3/Screenshot%202026-09-26%20013206.png)
![Truy cập S3 Website Endpoint](/images/5-Workshop/5.3/5.3.3/Screenshot%202026-09-26%20014405.png)
![Truy cập S3 Website Endpoint](/images/5-Workshop/5.3/5.3.3/Screenshot%202026-09-26%20014423.png)