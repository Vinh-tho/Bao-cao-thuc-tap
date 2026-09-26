---
title: "Kiểm vết với CloudTrail"
date: 2026-09-24
weight: 3
chapter: false
pre: " <b> 5.6.3. </b> "
---

# 5.6.3. Kích hoạt AWS CloudTrail để kiểm vết API

Trong kiến trúc hệ thống doanh nghiệp, tính bảo mật và khả năng truy vết lịch sử thao tác (Audit trail) đóng vai trò cốt lõi. Dịch vụ AWS CloudTrail được tích hợp nhằm mục đích ghi lại toàn bộ các lời gọi API và các thao tác thay đổi tài nguyên trên tài khoản AWS, hỗ trợ công tác quản trị, giám sát bảo mật và điều tra sự cố.

### Bước 1: Khởi tạo CloudTrail

1. Truy cập dịch vụ **CloudTrail** trên giao diện AWS Console.
2. Tại màn hình chính, chọn **Create trail** (Tạo vết). Hệ thống sẽ chuyển hướng sang giao diện cấu hình nhanh **Quick trail create**.
3. **Trail name**: `Eshop-Audit-Trail`.
4. Tại khu vực **Trail log bucket and folder**, hệ thống sẽ tự động khởi tạo một S3 bucket riêng biệt để chứa tệp nhật ký với định dạng tiêu chuẩn (ví dụ: `aws-cloudtrail-logs-...`).
5. Kiểm tra lại thông tin cấu hình và nhấn nút **Create trail** ở góc dưới cùng bên phải để hoàn tất khởi tạo.

![Khởi tạo CloudTrail](/images/5-Workshop/5.6/5.6.3/Screenshot%202026-09-26%20233616.png)
![Khởi tạo CloudTrail](/images/5-Workshop/5.6/5.6.3/Screenshot%202026-09-26%20233802.png)
![Khởi tạo CloudTrail](/images/5-Workshop/5.6/5.6.3/Screenshot%202026-09-26%20234024.png)

### Bước 2: Kiểm tra lịch sử sự kiện (Event history)

1. Tại thanh điều hướng bên trái của CloudTrail, chọn **Event history**.
2. Hệ thống hiển thị danh sách toàn bộ các hành động API quản trị đã thực thi trên tài khoản trong thời gian gần nhất.
3. Tại đây, quan sát các bản ghi sự kiện tự động được ghi nhận như `CreateTrail`, `CreateBucket`, `StartLogging` ứng với các thao tác cấu hình vừa thực hiện.
4. Kiểm tra chi tiết các trường thông tin được hệ thống lưu trữ bao gồm: tên sự kiện (`Event name`), thời điểm thực thi (`Event time`), định danh người dùng (`User name` hiển thị `root`), nguồn dịch vụ (`Event source`) và tên tài nguyên liên quan (`Resource name`).

![Xem Event History trên CloudTrail](/images/5-Workshop/5.6/5.6.3/Screenshot%202026-09-26%20234317.png)

**Kết luận:** Việc kích hoạt và kiểm tra trên AWS CloudTrail xác nhận hệ thống đã tự động ghi vết thành công mọi lời gọi API quản trị. Điều này đáp ứng các yêu cầu tuân thủ bảo mật, giúp nhà quản trị dễ dàng theo dõi biến động hạ tầng và đảm bảo tính minh bạch trong toàn bộ quá trình vận hành dự án E-shop.