---
title: "Kiểm vết với CloudTrail"
date: 2026-09-24
weight: 3
chapter: false
pre: " <b> 5.6.3. </b> "
---

# 5.6.3. Kích hoạt AWS CloudTrail để kiểm vết API

Trong môi trường doanh nghiệp, tính bảo mật và khả năng truy vết (Audit) là bắt buộc. Nếu một ngày hệ thống bị sập do ai đó lỡ tay xóa mất Security Group hoặc sửa cấu hình Load Balancer, bạn cần biết chính xác **Ai đã làm, làm lúc nào, và từ địa chỉ IP nào**. **AWS CloudTrail** chính là "camera an ninh" ghi lại mọi lời gọi API trong tài khoản AWS của bạn.

### Bước 1: Khởi tạo CloudTrail

1. Từ AWS Console, tìm và truy cập dịch vụ **CloudTrail**.
2. Tại màn hình chính, nhấn nút **Create trail** (Tạo vết).
3. **Trail name**: `Eshop-Audit-Trail`.
4. Tại phần **Storage location**:
   - Chọn **Create new S3 bucket**. AWS sẽ tự động tạo một Bucket mới có định dạng tên `aws-cloudtrail-logs-...` để chứa các file log.
   - Bỏ tick mục *Log file SSE-KMS encryption* (để đơn giản hóa trong khuôn khổ Workshop).
5. Cuộn xuống và nhấn **Next**.
6. Tại phần **Choose log events**, giữ nguyên tùy chọn mặc định là **Management events** (Ghi lại các thao tác như tạo, sửa, xóa tài nguyên). Nhấn **Next**.
7. Xem lại thông tin ở trang Review và nhấn **Create trail**.

![Khởi tạo CloudTrail](/images/5-Workshop/5.6.3/create_cloudtrail.png)

### Bước 2: Kiểm tra lịch sử sự kiện (Event history)

1. Nhìn sang menu bên trái của CloudTrail, chọn **Event history**.
2. Tại đây, bạn sẽ thấy danh sách mọi hành động vừa diễn ra trên tài khoản của mình.
3. Để thử nghiệm, bạn hãy thử mở một tab mới, vào EC2 và **tạo thử một Security Group rỗng**, hoặc thay đổi thông số một Auto Scaling Group.
4. Khoảng 5-10 phút sau, quay lại trang Event history của CloudTrail và tải lại. Bạn sẽ thấy bản ghi chi tiết (Event name: `CreateSecurityGroup`, User name: `Tên_IAM_User_của_bạn`, Source IP...).

![Xem Event History trên CloudTrail](/images/5-Workshop/5.6.3/cloudtrail_event_history.png)

Việc bật CloudTrail là tiêu chuẩn vàng (Best Practice) đầu tiên mà bất kỳ Quản trị viên hệ thống nào cũng phải làm khi nhận bàn giao một tài khoản AWS mới.