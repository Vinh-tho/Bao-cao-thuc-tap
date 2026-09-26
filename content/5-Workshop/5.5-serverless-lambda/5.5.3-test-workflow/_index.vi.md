---
title: "Kiểm thử luồng Event"
date: 2026-09-24
weight: 3
chapter: false
pre: " <b> 5.5.3. </b> "
---

# 5.5.3. Kiểm thử luồng tải ảnh và tự động xử lý (Resize)

Mục này trình bày quy trình kiểm thử và nghiệm thu luồng xử lý sự kiện tự động (Event-Driven) giữa Amazon S3 và AWS Lambda.

### Bước 1: Tải đối tượng mẫu lên S3

1. Tại giao diện dịch vụ **S3**, truy cập bucket lưu trữ `eshop-media-eshop-web`.
2. Di chuyển đến thẻ **Objects**.
3. Chọn **Upload** và tiến hành tải lên một tệp hình ảnh mẫu (ví dụ: `Hinh-nen-Full-HD-1080-cho-may-tinh-dep.jpg`).
4. Nhấn **Upload** và chờ hệ thống xác nhận tải lên thành công.

![Tải đối tượng mẫu lên S3](/images/5-Workshop/5.5/5.5.3/Screenshot%202026-09-26%20224451.png)

### Bước 2: Giám sát và nghiệm thu luồng sự kiện trên CloudWatch (Audit)

Do mã nguồn Lambda hiện tại được thiết lập ở chế độ mô phỏng (mock code) nhằm mục đích kiểm thử luồng tích hợp, hệ thống sẽ không khởi tạo thư mục `resized/` vật lý trên S3. Thay vào đó, toàn bộ quy trình tiếp nhận và xử lý sự kiện sẽ được ghi nhận vào nhật ký hệ thống. Quá trình nghiệm thu được thực hiện như sau:

1. Truy cập dịch vụ **CloudWatch** trên giao diện AWS Console.
2. Tại thanh điều hướng bên trái, di chuyển đến mục **Logs** và chọn **Log Management**.
3. Truy cập vào nhóm log có tên `/aws/lambda/Eshop-Image-Resizer`.
4. Chọn luồng log (log stream) mới nhất vừa được hệ thống khởi tạo.
5. Kiểm tra các bản ghi sự kiện (log events). Kết quả thực thi thành công sẽ ghi nhận chuỗi log xác nhận quá trình xử lý khớp với tệp tin vừa tải lên:
   - `INFO Sự kiện nhận được từ S3: {...}`
   - `INFO Đang xử lý đối tượng: Hinh-nen-Full-HD-1080-cho-may-tinh-dep.jpg từ bucket: eshop-media-eshop-web`
   - `INFO Xử lý hoàn tất, đối tượng mới được quy hoạch tại: resized/Hinh-nen-Full-HD-1080-cho-may-tinh-dep.jpg`

![Xem Log Lambda trên CloudWatch](/images/5-Workshop/5.5/5.5.3/Screenshot%202026-09-26%20225857.png)
![Xem Log Lambda trên CloudWatch](/images/5-Workshop/5.5/5.5.3/Screenshot%202026-09-26%20225910.png)

**Kết luận:** Việc nghiệm thu thành công thông qua nhật ký hệ thống minh chứng cho tính khả thi của cơ chế Serverless Event-Driven. Luồng tích hợp giữa S3 và Lambda đã hoạt động ổn định, đảm bảo khả năng tiếp nhận sự kiện theo thời gian thực và sẵn sàng cho việc tích hợp các thư viện xử lý ảnh chuyên sâu (như Sharp) ở các giai đoạn phát triển tiếp theo của dự án.
