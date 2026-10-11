---
title: "Worklog Tuần 2"
date: 2026-08-08
weight: 2
chapter: false
pre: " <b> 1.2. </b> "
---

### Mục tiêu tuần 2:

- Tìm hiểu các dịch vụ cốt lõi của AWS: EC2, S3 và IAM.
- Thực hành tạo máy ảo, lưu trữ đối tượng và host website tĩnh trên S3.
- Hiểu IAM User, Role, Policy và nguyên tắc đặc quyền tối thiểu.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| Thứ 7 – CN | Học EC2: instance type, AMI, EBS, key pair, khái niệm Security Group cơ bản và lựa chọn Free Tier (t2.micro). | 08/08/2026 | 09/08/2026 | <https://cloudjourney.awsstudygroup.com/> |
| Thứ 2 – Thứ 3 | Tạo EC2 t2.micro, kết nối SSH, cài web server đơn giản, sau đó stop/terminate để tiết kiệm chi phí. | 10/08/2026 | 11/08/2026 | <https://cloudjourney.awsstudygroup.com/> |
| Thứ 4 | Học S3: bucket, object, storage class, Block Public Access và Bucket Policy. | 12/08/2026 | 12/08/2026 | <https://cloudjourney.awsstudygroup.com/> |
| Thứ 5 | Tạo S3 bucket, upload tệp, bật Static Website Hosting (index/error document) và publish trang mẫu; xử lý lỗi 403 bằng cách tắt Block Public Access và thêm policy GetObject. | 13/08/2026 | 13/08/2026 | <https://cloudjourney.awsstudygroup.com/> |
| Thứ 6 | Học IAM User, Group, Role, Policy; tạo IAM Role cho dịch vụ AWS và gắn managed policy theo nguyên tắc đặc quyền tối thiểu. | 14/08/2026 | 14/08/2026 | <https://docs.aws.amazon.com/IAM/latest/UserGuide/> |

### Chi tiết cấu hình kỹ thuật:

| Thành phần | Cấu hình / giá trị |
| --- | --- |
| EC2 instance | t2.micro (nằm trong Free Tier), truy cập SSH bằng key pair trong bài lab |
| Bucket website S3 | Tắt Block Public Access; Static website hosting: index document index.html, error document error.html; Object Ownership: ACLs disabled |
| Bucket Policy | Effect Allow, Principal *, Action s3:GetObject, Resource arn:aws:s3:::<tên-bucket>/* |
| Eshop-EC2-Instance-Role | Trusted entity EC2; policy AmazonEC2ContainerServiceforEC2Role |
| Eshop-ECS-Task-Execution-Role | Trusted entity ECS Task; policy AmazonECSTaskExecutionRolePolicy |
| Eshop-Lambda-Image-Role | Trusted entity Lambda; policy AWSLambdaBasicExecutionRole và AmazonS3FullAccess (đơn giản hóa cho thực hành) |

### Kiến thức đã học:

- Cơ bản về EC2: AMI (ảnh hệ điều hành), instance type (CPU/RAM), key pair (xác thực SSH) và Security Group (tường lửa ảo).
- S3 là lưu trữ đối tượng: tên bucket duy nhất toàn cầu, chữ thường, không khoảng trắng; đối tượng được định danh bằng key; static hosting phân phối tệp qua HTTP từ website endpoint.
- Bucket policy là JSON gồm Version, Statement, Effect, Principal, Action và Resource; hậu tố /* là bắt buộc để áp dụng cho các object trong bucket.
- IAM User đại diện cho người dùng với thông tin xác thực dài hạn; IAM Role được dịch vụ đảm nhận tạm thời, nên dịch vụ không cần access key tĩnh.
- Đặc quyền tối thiểu: chỉ cấp những quyền mà thành phần cần (role Lambda dùng S3FullAccess chỉ để đơn giản hóa bài lab, thực tế cần thu hẹp).

### Lệnh và mã đã sử dụng:

**Ví dụ lệnh SSH dùng trong bài lab EC2**

```bash
ssh -i <key-pair>.pem ec2-user@<public-ip>
```

**Bucket policy cho phép đọc công khai các tệp website**

```json
{
  "Version": "2012-10-17",
  "Statement": [{
    "Sid": "PublicReadGetObject",
    "Effect": "Allow",
    "Principal": "*",
    "Action": "s3:GetObject",
    "Resource": "arn:aws:s3:::<bucket-name>/*"
  }]
}
```

### Khó khăn và cách giải quyết:

- Website S3 báo 403 Forbidden. → Tắt Block Public Access và bổ sung Bucket Policy với Resource có hậu tố /* .

### Kết quả đạt được tuần 2:

- Tạo và kết nối thành công EC2.
- Host được website tĩnh trên S3 và hiểu vì sao bucket trả lỗi 403 khi thiếu policy phù hợp.
- Phân biệt IAM User và IAM Role, hiểu vì sao dịch vụ nên dùng role thay cho access key tĩnh.
- Đặt nền tảng cho bucket frontend và các IAM Role của dự án E-shop.

### Sản phẩm và minh chứng:

- Phiên SSH vào EC2 hoạt động.
- Website tĩnh trên S3 truy cập được qua website endpoint.
- Ba IAM Role được định nghĩa để dùng ở các tuần sau (EC2 instance, ECS task execution, Lambda).

### Kế hoạch tuần tiếp theo:

- Học mạng VPC và xây dựng Public/Private Subnet.
