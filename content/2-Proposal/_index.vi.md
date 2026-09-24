---
title: "Bản đề xuất"
date: 2026-09-24
weight: 2
chapter: false
pre: "<b> 2. </b>"
---

# Hệ thống Web E-shop Mở rộng linh hoạt và Tối ưu trên AWS

## Kiến trúc Container trên nền EC2 & Event-Driven cho Nền tảng Thương mại Điện tử

### 1. Tóm tắt điều hành

Đề xuất này trình bày giải pháp kiến trúc tổng thể triển khai dự án **Web E-shop** trên hạ tầng đám mây AWS.

**Kiến trúc chủ đạo:** Hệ thống được thiết kế theo mô hình kết hợp giữa **Static Hosting**, **Container hóa trên nền EC2** và **Serverless**: Frontend và tài nguyên tĩnh được lưu trữ trên **Amazon S3**; Backend được đóng gói bằng **Docker** và triển khai trên **Amazon ECS chạy trên các EC2 instance** (EC2 Auto Scaling Group kết hợp ECS Capacity Provider); các tác vụ xử lý nền được thực hiện bằng **AWS Lambda**. Toàn bộ hệ thống được đặt trong **VPC**, phân tách rõ **Public Subnet** (ALB) và **Private Subnet** (EC2/ECS Backend), đồng thời được giám sát bởi **CloudWatch & CloudTrail**.

Thay vì chạy toàn bộ ứng dụng trên một máy chủ truyền thống (Monolithic), dự án sử dụng kiến trúc phân tách Frontend/Backend, container hóa Backend và tự động hóa tác vụ phụ trợ (xử lý ảnh sản phẩm) bằng Lambda để giảm tải cho máy chủ chính.

---

### 2. Tuyên bố vấn đề

#### Vấn đề của các hệ thống E-shop truyền thống

- **Không chịu được tải đột biến (Traffic Spikes):** Trong các dịp Flash Sale hoặc khuyến mãi lớn, lượng truy cập tăng đột biến dễ làm sập máy chủ web nếu cấu hình cố định, dẫn đến mất doanh thu và trải nghiệm xấu cho khách hàng.
- **Chi phí duy trì không tối ưu:** Phải thuê máy chủ cấu hình dư thừa để phòng hờ những lúc cao điểm, gây lãng phí tài nguyên vào những ngày bình thường hoặc ban đêm khi ít khách mua hàng.
- **Hiệu suất tải trang tĩnh kém:** Việc bắt máy chủ (Backend) phải xử lý và trả về cả các file giao diện (HTML/CSS/JS) và hình ảnh sản phẩm làm giảm tốc độ xử lý các giao dịch cốt lõi (như giỏ hàng, thanh toán).
- **Khó khăn trong việc cập nhật (Deployment):** Việc cập nhật tính năng mới trên kiến trúc máy chủ cố định thường phức tạp và tiềm ẩn rủi ro gián đoạn dịch vụ.

#### Giải pháp đề xuất

Hệ thống được tái cấu trúc thành các thành phần độc lập (Decoupled Architecture), áp dụng các dịch vụ cốt lõi của AWS:

1. **Frontend trên Amazon S3:** Giao diện người dùng (Web Client) và hình ảnh sản phẩm được lưu trữ dưới dạng tài nguyên tĩnh trên **Amazon S3**, giúp tải trang nhanh, có khả năng mở rộng và chi phí thấp.
2. **Backend Containerization trên nền EC2:** Backend API (xử lý logic giỏ hàng, thanh toán, quản lý sản phẩm) được đóng gói bằng **Docker**, chạy trên **Amazon ECS với EC2 launch type**. Các EC2 instance làm nền cho ECS được quản lý bởi **EC2 Auto Scaling Group**, kết hợp **ECS Capacity Provider** để đảm bảo đủ tài nguyên chạy Task. Phía trước là **Application Load Balancer (ALB)** phân phối request tới các Task thông qua Target Group.
3. **Background Processing với Lambda:** Khi Admin upload ảnh sản phẩm lên S3, sự kiện này kích hoạt **AWS Lambda** tự động xử lý (resize) ảnh — một ví dụ điển hình của kiến trúc Event-driven Serverless.
4. **Bảo mật & Giám sát:** Toàn bộ Backend (EC2 + ECS) được đặt trong **Private Subnet** của **VPC**, không mở trực tiếp ra Internet — chỉ nhận lưu lượng đã qua ALB ở Public Subnet. Mọi hoạt động được ghi log bởi **CloudTrail** và giám sát bằng **CloudWatch**.

#### Lợi ích kỳ vọng

- **Khả năng mở rộng linh hoạt khi lưu lượng truy cập tăng:** ECS Service Auto Scaling điều chỉnh số lượng Task; khi năng lực EC2 không đủ, ECS Capacity Provider phối hợp với EC2 Auto Scaling Group để bổ sung EC2 instance.
- **Có khả năng tối ưu chi phí:** S3 và Lambda áp dụng mô hình tính phí theo mức sử dụng thực tế; EC2 Auto Scaling Group cho phép điều chỉnh số lượng instance theo nhu cầu thay vì duy trì cố định một số lượng lớn máy chủ.
- **Bảo mật dữ liệu khách hàng:** Cách ly máy chủ xử lý dữ liệu khỏi Internet công cộng, giới hạn quyền truy cập thông qua Security Groups và IAM.

---

### 3. Kiến trúc giải pháp

#### 3.1. Hiện trạng vs. Kiến trúc mục tiêu

Vì hệ thống hiện tại **chưa được kết nối lên AWS**, phần này làm rõ ranh giới giữa hiện trạng và mục tiêu đề xuất.

**Hiện trạng:**

```text
Web E-shop
    │
    ▼
 Backend
    │
    ▼
 Database
```

**Kiến trúc mục tiêu (đề xuất triển khai trên AWS):**

```text
                         INTERNET
                             │
              ┌──────────────┴──────────────┐
              │                             │
              ▼                             ▼
        Amazon S3                    Application Load
   Frontend + Media                     Balancer
                                            │
                                            ▼
                                  ┌──────────────────┐
                                  │   PUBLIC SUBNET  │
                                  │       ALB        │
                                  └────────┬─────────┘
                                           │
                                           ▼
                                  ┌──────────────────┐
                                  │  PRIVATE SUBNET  │
                                  │                  │
                                  │   ECS Cluster    │
                                  │       │          │
                                  │   EC2 Instances  │
                                  │       │          │
                                  │  Docker Backend  │
                                  │                  │
                                  │    Database      │
                                  └──────────────────┘
                                           ▲
                                           │
                              EC2 Auto Scaling Group
                                           ▲
                                           │
                                  ECS Capacity Provider

        Amazon S3 (Media)
              │
              │ Event Notification
              ▼
         AWS Lambda
              │
              ▼
       Image Processing
              │
              ▼
        Amazon S3 (Media)


     CloudWatch ─────── Monitoring / Logs / Alarms
     CloudTrail ─────── AWS API Audit
     IAM ────────────── Access Control
```

#### 3.2. Sơ đồ kiến trúc tổng thể

![Sơ đồ kiến trúc E-shop](/images/2-Proposal/eshop_architecture.png)

#### 3.3. Phân chia các luồng xử lý chính (4 Flows)

##### Flow F — Frontend & Trải nghiệm khách hàng

```text
F1: User truy cập E-shop
        ↓
F2: Amazon S3 cung cấp các tài nguyên Frontend tĩnh
    (HTML / CSS / JavaScript / hình ảnh)
        ↓
F3: Browser gọi Backend API thông qua Application Load Balancer
        ↓
F4: Backend xử lý nghiệp vụ và trả dữ liệu về Client
```

##### Flow B — Backend API & Xử lý giao dịch (Core Business)

```text
B1: Client gửi API Request
        ↓
B2: Application Load Balancer (ALB) tiếp nhận
        ↓
B3: ALB phân phối Request tới ECS Service thông qua Target Group
        ↓
B4: ECS Service chuyển Request tới Docker Container đang chạy trên EC2
        ↓
B5: Backend xử lý logic nghiệp vụ
        ↓
B6: Ghi/đọc Database hoặc S3
```

**Cơ chế mở rộng (Auto Scaling) đi kèm:**

```text
Traffic tăng
    ↓
ECS Service Auto Scaling
    ↓
Tăng số lượng ECS Task
    ↓
Nếu EC2 không đủ capacity
    ↓
ECS Capacity Provider
    ↓
EC2 Auto Scaling Group
    ↓
Tạo thêm EC2 instance
```

> **Lưu ý:** ALB chịu trách nhiệm phân phối request; ECS Service Auto Scaling điều chỉnh số lượng Task/Container; EC2 Auto Scaling Group chỉ điều chỉnh số lượng EC2 instance làm nền cho ECS.

##### Flow E — Tác vụ nền theo sự kiện (Event-driven Serverless)

```text
E1: Admin upload ảnh sản phẩm lên S3
        ↓
E2: S3 Event Notification
        ↓
E3: AWS Lambda được kích hoạt
        ↓
E4: Resize ảnh (Thumbnail / Medium / Large)
        ↓
E5: Lưu kết quả vào S3
```

##### Flow S — Bảo mật & Giám sát

```text
S1: EC2 / ECS / Lambda phát sinh log & metric
        ↓
S2: CloudWatch thu thập Log, Metrics
        ↓
S3: Dashboard / Alarm (cảnh báo qua email khi lỗi HTTP 500 tăng cao)

AWS API Calls
      ↓
  CloudTrail
      ↓
  Audit Log
```

- **IAM:** Áp dụng nguyên tắc quyền tối thiểu — ví dụ ECS Task Role chỉ được đọc/ghi vào S3 Bucket ảnh sản phẩm, EC2 instance role không được cấp quyền quản trị không cần thiết.

---

### 4. Triển khai kỹ thuật

#### Các giai đoạn triển khai

1. **Giai đoạn 1: Xây dựng Nền tảng Mạng (Networking) & Bảo mật**
   - Tạo **VPC** riêng biệt cho dự án.
   - Chia mạng thành Public Subnets (dành cho ALB) và Private Subnets (dành cho EC2/ECS Backend và Database).
   - Cấu hình **Internet Gateway (IGW)**, NAT Gateway và định tuyến (Route Tables).
   - Tạo các **IAM Roles** và Security Groups theo nguyên tắc quyền tối thiểu.

2. **Giai đoạn 2: Lưu trữ Frontend và Tài sản số (Storage)**
   - Cấu hình **Amazon S3** để lưu trữ web tĩnh.
   - Tạo S3 Bucket thứ hai để lưu trữ Media (hình ảnh, video sản phẩm).

3. **Giai đoạn 3: Container hóa và Triển khai Backend trên nền EC2**
   - Viết `Dockerfile` đóng gói mã nguồn Backend.
   - Tạo **Launch Template** và **EC2 Auto Scaling Group** làm nền tảng chạy container.
   - Tạo **ECS Cluster (EC2 launch type)**, Task Definitions và cấu hình **ECS Capacity Provider** gắn với Auto Scaling Group.
   - Cấu hình **Application Load Balancer (ALB)** và Target Group kết nối tới ECS Service.
   - Thiết lập **ECS Service Auto Scaling** dựa trên chỉ số CPU/traffic.

4. **Giai đoạn 4: Tích hợp Serverless và Tự động hóa**
   - Viết code **AWS Lambda** (Node.js/Python) để xử lý ảnh sản phẩm.
   - Thiết lập S3 Event Notification để tự động kích hoạt Lambda khi có file mới.

5. **Giai đoạn 5: Giám sát Hệ thống (Monitoring)**
   - Đẩy log từ EC2/ECS lên **CloudWatch Logs**.
   - Cài đặt CloudWatch Alarms cảnh báo lỗi hoặc khi tài nguyên (CPU) sắp cạn kiệt.
   - Bật **CloudTrail** để kiểm vết hạ tầng.

---

### 5. Lộ trình & Mốc triển khai

```text
+-----------------------------------------------------------------------------------+
| Tuần 1: Thiết lập Kiến trúc mạng & Lưu trữ tĩnh                                   |
|   - Tạo VPC, Public/Private Subnets, Internet Gateway, Security Groups.           |
|   - Thiết lập S3 Hosting cho Frontend và S3 cho Media Storage.                     |
|   - Tạo các IAM Policy cần thiết.                                                  |
+-----------------------------------------------------------------------------------+
                                      |
                                      v
+-----------------------------------------------------------------------------------+
| Tuần 2-3: EC2, Docker, ECS & Cân bằng tải (Core Backend)                           |
|   - Tạo Launch Template & EC2 Auto Scaling Group.                                  |
|   - Build Docker Image cho Backend.                                                |
|   - Thiết lập ECS Cluster (EC2 launch type) & ECS Capacity Provider.               |
|   - Cấu hình Application Load Balancer (ALB), Target Group.                        |
|   - Thiết lập ECS Service Auto Scaling và EC2 Auto Scaling Group.                  |
+-----------------------------------------------------------------------------------+
                                      |
                                      v
+-----------------------------------------------------------------------------------+
| Tuần 4: Serverless, Tích hợp hoàn thiện & Giám sát                                 |
|   - Viết và triển khai hàm AWS Lambda xử lý ảnh sản phẩm.                          |
|   - Kết nối Event Trigger từ S3 tới Lambda.                                        |
|   - Xây dựng CloudWatch Dashboard, cấu hình Alarms.                                |
|   - Kiểm thử toàn hệ thống (Load testing giả lập Flash Sale).                      |
+-----------------------------------------------------------------------------------+
```

---

### 6. Ước tính ngân sách (Kiến trúc tiêu chuẩn)

Kiến trúc kết hợp giữa **EC2/ECS và Serverless** mang lại độ ổn định cao với chi phí linh hoạt:

| Dịch vụ AWS                   | Mục đích sử dụng / Quy mô ước tính             | Phân bổ chi phí / Tính chất                                                                         |
| ----------------------------- | ---------------------------------------------- | --------------------------------------------------------------------------------------------------- |
| **Amazon S3**                 | Lưu trữ tĩnh Frontend và hình ảnh sản phẩm     | Rất thấp (Pay per GB)                                                                               |
| **Amazon VPC**                | VPC, Subnet, IGW, Security Groups, NAT Gateway | VPC cơ bản không tính phí; NAT Gateway và một số thành phần mạng liên quan có thể phát sinh chi phí |
| **Application Load Balancer** | Phân phối luồng truy cập vào Backend           | Chi phí cố định hàng giờ + lượng data xử lý                                                         |
| **Amazon EC2 + Amazon ECS**   | EC2 instance làm nền, chạy Docker container    | Chi phí theo loại instance và số giờ chạy                                                           |
| **AWS Lambda**                | Resize ảnh sản phẩm khi có sự kiện upload      | Trả tiền theo số lần gọi                                                                            |
| **CloudWatch / CloudTrail**   | Lưu trữ Log và Cảnh báo                        | Chủ yếu theo dung lượng Log lưu trữ                                                                 |

> **Lưu ý:** Chi phí ước tính tham khảo, phụ thuộc vào Region, cấu hình tài nguyên (loại EC2 instance, số lượng), thời gian chạy và lưu lượng sử dụng thực tế.

---

### 7. Đánh giá rủi ro

| Rủi ro tiềm ẩn                                 | Mức độ     | Chiến lược giảm thiểu                                                                                                                           |
| ---------------------------------------------- | ---------- | ----------------------------------------------------------------------------------------------------------------------------------------------- |
| **Khách hàng ồ ạt truy cập (Flash Sale)**      | Cao        | ALB kết hợp ECS Service Auto Scaling và EC2 Auto Scaling Group có thể tự động điều chỉnh số lượng Task/EC2 instance dựa trên chỉ số tải.        |
| **Lỗi Container / Mã nguồn bị Crash**          | Trung bình | ECS có thể phát hiện task/container không khỏe thông qua Health Check và khởi động task thay thế, giúp duy trì khả năng phục vụ của hệ thống.   |
| **Nguy cơ tấn công trực tiếp vào DB/Backend**  | Cao        | Backend (EC2/ECS) chạy trong Private Subnet, chỉ nhận lưu lượng đã qua ALB ở Public Subnet. Database cũng không được mở trực tiếp cho Internet. |
| **Lỗi hoặc mất dữ liệu Log**                   | Trung bình | Sử dụng CloudWatch Logs để tập trung log và CloudTrail để theo dõi hoạt động API trên AWS.                                                      |
| **Chi phí tăng đột biến (do EC2 scale nhiều)** | Trung bình | Thiết lập CloudWatch monitoring, giới hạn số lượng instance tối đa trong Auto Scaling Group và cảnh báo chi phí (AWS Budgets).                  |

---

### 8. Kết quả kỳ vọng

1. **Triển khai thành công Web E-shop trên AWS:** Đưa hệ thống Web E-shop hiện tại lên môi trường AWS và kết nối các thành phần Frontend, Backend và lưu trữ theo kiến trúc được đề xuất.
2. **Vận dụng kiến thức AWS:** Áp dụng các kiến thức về **EC2, S3, IAM, VPC, Lambda, CloudWatch, CloudTrail, ELB, Auto Scaling, ECS và Docker** vào một hệ thống thực tế.
3. **Khả năng mở rộng:** Backend có khả năng mở rộng số lượng ECS Task khi lưu lượng truy cập tăng thông qua ECS Service Auto Scaling; khi capacity của EC2 không đủ, ECS Capacity Provider phối hợp với EC2 Auto Scaling Group để bổ sung EC2 instance.
4. **Tự động hóa:** AWS Lambda xử lý tác vụ nền (xử lý ảnh sản phẩm) theo sự kiện từ S3, giảm tải cho Backend chính.
5. **Nền tảng cho mở rộng tương lai:** Kiến trúc cho phép bổ sung sau này các thành phần như Amazon CloudFront, Amazon RDS/Aurora, Amazon SES, Amazon SQS hoặc quy trình CI/CD mà không cần thay đổi toàn bộ hệ thống.
