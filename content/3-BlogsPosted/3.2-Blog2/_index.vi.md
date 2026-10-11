---
title: "Blog 2"
date: 2026-07-14
weight: 1
chapter: false
pre: " <b> 3.2. </b> "
---

# Cách LithoLens của ALS Geoanalytics cách mạng hóa quy trình ghi log mẫu lõi bằng học máy với Amazon EKS

Trong ngành khai thác mỏ và địa chất, **core logging** là công đoạn quan trọng để ghi nhận đặc điểm của mẫu lõi khoan. Các chuyên gia cần xem xét hình ảnh và mô tả nhiều thuộc tính như màu sắc, thành phần, cấu trúc, mức độ phong hóa và các dấu hiệu địa chất khác. Khi số lượng mẫu tăng lên, việc xử lý thủ công trở nên tốn thời gian, khó mở rộng và có thể tạo ra sự không nhất quán giữa các lần đánh giá.

**LithoLens của ALS Geoanalytics** minh họa cách học máy và điện toán đám mây có thể hỗ trợ quy trình này. Thay vì chỉ lưu trữ hình ảnh như một kho dữ liệu, hệ thống tiếp nhận ảnh mẫu lõi, đưa chúng qua các container xử lý ML và lưu lại kết quả cùng siêu dữ liệu liên quan. Với **Amazon Elastic Kubernetes Service (Amazon EKS)** làm nền tảng chạy workload container, quy trình có thể xử lý nhiều ảnh song song và rút ngắn thời gian từ vài tuần xuống còn vài giờ, tùy thuộc vào khối lượng và cấu hình xử lý.

## Từ ghi log thủ công đến quy trình số hóa

Quy trình truyền thống thường bao gồm chụp ảnh mẫu lõi, sắp xếp dữ liệu, ghi nhận thông tin và để chuyên gia đánh giá từng phần. Cách làm này có giá trị chuyên môn cao nhưng dễ gặp các hạn chế:

- khối lượng ảnh lớn làm tăng thời gian chờ;
- dữ liệu hình ảnh và thông tin dự án có thể bị phân tán;
- khó tăng công suất xử lý trong giai đoạn có nhiều mẫu;
- việc lặp lại các bước tiền xử lý tốn công;
- cần cơ chế theo dõi để biết một ảnh đang chờ, đang xử lý hay đã hoàn thành.

LithoLens đưa các bước này vào một pipeline có thể theo dõi. Hình ảnh được tải lên qua API, lưu trữ bền vững, xử lý bởi các workload ML trên EKS và liên kết với thông tin dự án trong cơ sở dữ liệu quan hệ.

## Kiến trúc tổng quan

![Kiến trúc LithoLens trên AWS](/images/3-BlogsPosted/blog2.png)

Kiến trúc có thể được chia thành ba lớp:

1. **API và xác thực**: API Gateway, AWS Lambda và Amazon Cognito cung cấp điểm vào bảo mật cho ứng dụng.
2. **Tính toán và xử lý**: Amazon EKS chạy các container chứa mô hình và các bước xử lý ảnh.
3. **Lưu trữ, dữ liệu và quan sát**: Amazon S3 lưu ảnh và kết quả; Amazon RDS quản lý metadata; CloudWatch ghi log và metric.

Việc phân tách các lớp giúp hệ thống dễ mở rộng độc lập. Dung lượng ảnh có thể tăng mà không nhất thiết phải thay đổi mô hình dữ liệu; công suất xử lý có thể điều chỉnh mà không phải thay đổi giao diện tải ảnh.

## Luồng xử lý một bộ ảnh mẫu lõi

Một luồng xử lý điển hình gồm các bước:

1. Người dùng đăng nhập và được xác thực bởi Amazon Cognito.
2. Ứng dụng gửi yêu cầu tải ảnh hoặc tạo một job xử lý thông qua API Gateway.
3. API Gateway chuyển yêu cầu đến Lambda để kiểm tra danh tính, metadata và các điều kiện đầu vào.
4. Ảnh gốc được lưu trong Amazon S3, còn thông tin như project, lỗ khoan, mẫu và trạng thái job được ghi vào Amazon RDS.
5. Job xử lý được đưa đến workload phù hợp trên EKS.
6. Container ML đọc dữ liệu, thực hiện các bước tiền xử lý và suy luận theo pipeline của ứng dụng.
7. Kết quả được lưu lại trong S3; trạng thái và thông tin tra cứu được cập nhật trong RDS.
8. Log, metric và lỗi được gửi đến CloudWatch để đội vận hành theo dõi.

Việc lưu ảnh gốc tách biệt với metadata giúp giữ lại dữ liệu đầu vào để kiểm tra hoặc xử lý lại. Cơ sở dữ liệu quan hệ đóng vai trò là lớp điều phối và tra cứu, giúp người dùng biết kết quả thuộc dự án nào và job đang ở trạng thái nào.

## Vai trò của Amazon EKS

Amazon EKS cung cấp một control plane Kubernetes được quản lý, trong khi ứng dụng tập trung vào việc đóng gói và vận hành các container xử lý. Đây là lựa chọn phù hợp khi pipeline ML cần:

- chạy nhiều workload đồng thời;
- đóng gói môi trường phụ thuộc của mô hình;
- phân tách các thành phần xử lý thành các service hoặc job;
- điều chỉnh năng lực tính toán theo khối lượng ảnh;
- triển khai phiên bản mới của mô hình có kiểm soát.

Với workload xử lý ảnh, khả năng mở rộng là yếu tố quan trọng. Khi số lượng job tăng, hệ thống có thể bổ sung năng lực xử lý; khi nhu cầu giảm, năng lực dư thừa có thể được thu hẹp. Việc scale cần đi kèm giới hạn tài nguyên, hàng đợi công việc và cơ chế retry để một ảnh lỗi không làm dừng toàn bộ pipeline.

EKS cũng tạo ra một ranh giới rõ ràng giữa lớp API và lớp ML. API không cần trực tiếp thực hiện suy luận nặng; nó chỉ tiếp nhận yêu cầu và theo dõi trạng thái. Các container xử lý có thể được tối ưu, cập nhật hoặc mở rộng theo nhu cầu riêng.

## Amazon S3: lưu trữ ảnh và kết quả

Hình ảnh mẫu lõi thường có kích thước lớn và cần được giữ lại trong thời gian dài. Amazon S3 phù hợp cho việc lưu trữ:

- ảnh gốc chưa xử lý;
- ảnh hoặc dữ liệu trung gian;
- kết quả phân tích;
- artifact phục vụ kiểm tra và tái xử lý.

Nên tổ chức object key theo project, hole, sample và processing job để việc truy xuất có quy ước nhất quán. Metadata đi kèm cần được kiểm soát để liên kết chính xác giữa ảnh và bản ghi trong RDS. Quyền truy cập bucket cũng nên giới hạn theo vai trò, đồng thời cân nhắc versioning, mã hóa, lifecycle và chính sách lưu giữ phù hợp với yêu cầu dữ liệu của dự án.

## Amazon RDS: dữ liệu nghiệp vụ và siêu dữ liệu

Amazon RDS lưu các thông tin có cấu trúc như:

- dự án và người dùng;
- thông tin lỗ khoan, mẫu lõi và vị trí;
- trạng thái tải lên và trạng thái xử lý;
- phiên bản pipeline hoặc mô hình;
- đường dẫn đến object trên S3;
- kết quả tóm tắt và thông tin audit.

Tách metadata khỏi file ảnh giúp truy vấn danh sách job nhanh hơn và tránh phải quét toàn bộ object storage. Đồng thời, việc ghi nhận phiên bản xử lý giúp phân biệt kết quả được tạo bởi pipeline nào, một yếu tố quan trọng khi cần so sánh hoặc tái lập kết quả.

## Xác thực và bảo mật API

Amazon Cognito xác thực người dùng trước khi họ truy cập các chức năng của hệ thống. API Gateway là lớp tiếp nhận request, còn Lambda có thể thực hiện các kiểm tra bổ sung như:

- người dùng có thuộc project được yêu cầu hay không;
- file và metadata có đúng định dạng không;
- request có bị gửi lặp hay không;
- job có vượt giới hạn được phép không;
- người dùng có quyền xem ảnh hoặc kết quả tương ứng không.

Không nên xem việc đăng nhập thành công là đủ để cấp quyền truy cập mọi dữ liệu. Quyền cần được kiểm tra theo project, vai trò và thao tác. Ảnh mẫu lõi và kết quả địa chất có thể là dữ liệu nhạy cảm về mặt thương mại, vì vậy các log và response API cũng cần tránh để lộ nội dung không cần thiết.

## Quan sát hệ thống với Amazon CloudWatch

Một pipeline ML có nhiều điểm có thể thất bại: tải file, xác thực, lưu object, xếp hàng job, khởi động container, suy luận hoặc ghi kết quả. CloudWatch giúp tập trung log và metric để đội vận hành theo dõi:

- số lượng job chờ, đang chạy và hoàn thành;
- thời gian xử lý trung bình và các job vượt ngưỡng;
- tỷ lệ lỗi theo bước;
- mức sử dụng CPU, memory và năng lực cluster;
- lỗi của API, Lambda và container;
- tình trạng kết nối đến S3 và RDS.

Theo dõi thời gian xử lý theo từng giai đoạn giúp phân biệt bottleneck nằm ở upload, cơ sở dữ liệu hay workload ML. Cảnh báo cũng nên hướng đến hành động cụ thể, chẳng hạn queue tăng bất thường, tỷ lệ lỗi vượt ngưỡng hoặc cluster thiếu năng lực xử lý.

## Tác động đối với hoạt động địa chất

Khi pipeline có thể xử lý song song, thời gian chờ giảm đáng kể và các chuyên gia có thể tiếp cận kết quả sớm hơn. Số hóa cũng giúp dữ liệu được tìm kiếm, đối chiếu và chia sẻ dễ dàng hơn so với quy trình dựa trên file rời rạc.

Điều quan trọng là ML nên được xem như công cụ hỗ trợ chuyên gia, không phải thay thế hoàn toàn đánh giá địa chất. Kết quả cần có trạng thái, nguồn gốc và khả năng truy vết để người dùng có thể kiểm tra các trường hợp không chắc chắn hoặc yêu cầu xem xét thủ công.

## Các điểm cần cân nhắc khi triển khai

Một hệ thống tương tự cần chú ý đến:

- chuẩn hóa chất lượng, hướng chụp và metadata của ảnh đầu vào;
- quản lý phiên bản mô hình và pipeline;
- xử lý retry, timeout và dead-letter cho job lỗi;
- giới hạn kích thước file và tốc độ tải lên;
- bảo vệ dữ liệu trong quá trình truyền và lưu trữ;
- kiểm soát chi phí lưu trữ và compute;
- phân quyền theo project và vai trò;
- lưu lại audit trail cho các kết quả quan trọng.

Khả năng mở rộng của EKS chỉ mang lại hiệu quả khi pipeline được thiết kế cho xử lý bất đồng bộ và các job có thể chạy độc lập. Nếu API phải chờ một suy luận dài hoàn tất, trải nghiệm người dùng và khả năng chịu tải sẽ bị hạn chế.

## Kết luận

LithoLens cho thấy một quy trình giàu dữ liệu như core logging có thể được hiện đại hóa bằng cách kết hợp API bảo mật, lưu trữ đối tượng, cơ sở dữ liệu quan hệ và machine learning trên Kubernetes. Amazon EKS cung cấp nền tảng linh hoạt cho các container xử lý ảnh; S3 lưu trữ dữ liệu lớn; RDS giữ thông tin nghiệp vụ và metadata; Cognito, API Gateway và Lambda bảo vệ luồng truy cập; còn CloudWatch cung cấp khả năng quan sát.

Giá trị của kiến trúc không chỉ nằm ở việc chạy mô hình nhanh hơn. Khi dữ liệu đầu vào, job xử lý, kết quả và log được liên kết đầy đủ, tổ chức có một pipeline có thể mở rộng, truy vết và cải tiến liên tục—từ đó hỗ trợ các chuyên gia đưa ra quyết định địa chất nhanh hơn và dựa trên dữ liệu hơn.
