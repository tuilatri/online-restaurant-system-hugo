---
title : "Giới thiệu và Tổng quan kiến trúc"
date : "2025-09-06" 
weight : 1 
chapter : false
pre : " <b> 1. </b> "
---

### Tổng quan kiến trúc

Trong workshop này, chúng ta sẽ triển khai một ứng dụng nhà hàng online hoàn chỉnh lên môi trường AWS. Kiến trúc được thiết kế để đảm bảo **tính sẵn sàng cao (High Availability)**, **khả năng mở rộng (Scalability)**, và **bảo mật (Security)** bằng cách sử dụng các dịch vụ cốt lõi của AWS.

Dưới đây là sơ đồ kiến trúc tổng quan của dự án mà chúng ta sẽ xây dựng:

![Kiến trúc dự án Nhà hàng Online trên AWS](/images/1.introduction/ors-architecture.png) 

### Các dịch vụ AWS được sử dụng

Dưới đây là danh sách các dịch vụ chính và vai trò của chúng trong dự án này:

#### Mạng và Bảo mật (Networking & Security)

*   **Amazon VPC (Virtual Private Cloud)**: Là nền tảng mạng cho toàn bộ dự án. Chúng ta sẽ tạo một VPC tên `ors-vpc` để tạo ra một môi trường mạng riêng biệt, cho phép kiểm soát hoàn toàn dải IP, subnets, và các bảng định tuyến.

*   **Public & Private Subnets**:
    *   **Public Subnets**: Được sử dụng cho các tài nguyên cần truy cập từ Internet như **Application Load Balancer**.
    *   **Private Subnets**: Dùng để chứa các tài nguyên nhạy cảm như máy chủ backend **EC2** và cơ sở dữ liệu **RDS**, không cho phép truy cập trực tiếp từ Internet để tăng cường bảo mật.

*   **Security Groups**: Hoạt động như một tường lửa ảo để kiểm soát lưu lượng truy cập.
    *   `ors-sg`: Dành cho Web Server, cho phép traffic từ Internet vào cổng **80 (HTTP)**, **443 (HTTPS)** và **5000 (Node.js App)**, cũng như cổng **22 (SSH)** từ IP của bạn để quản trị.
    *   `ors-db-sg`: Dành cho Database, chỉ cho phép kết nối từ các máy chủ thuộc Security Group `ors-sg` trên cổng **3306 (MySQL)**, đảm bảo cơ sở dữ liệu được bảo vệ tuyệt đối.

#### Cơ sở dữ liệu (Database)

*   **Amazon RDS (Relational Database Service)**: Cung cấp một cơ sở dữ liệu **MySQL** được quản lý hoàn toàn. Chúng ta sẽ tạo một instance tên `ors-db` trong Private Subnet, giúp đơn giản hóa việc vận hành, sao lưu và đảm bảo an toàn.

#### Tính toán và Mở rộng (Compute & Scalability)

*   **Amazon EC2 (Elastic Compute Cloud)**: Cung cấp máy chủ ảo để chạy ứng dụng backend **Node.js**. Các máy chủ này sẽ được đặt trong Private Subnet.

*   **AMI (Amazon Machine Image) & Launch Template**:
    *   Chúng ta sẽ cấu hình một EC2 instance hoàn chỉnh (cài đặt Node.js, Git,...) rồi tạo một AMI tên `ors-ami`.
    *   AMI này sau đó được sử dụng trong một **Launch Template** tên `ors-launch-template` để làm "khuôn mẫu" cho việc tạo ra các máy chủ mới một cách tự động và đồng nhất.

*   **Application Load Balancer (ALB)**: Tự động phân phối lưu lượng truy cập từ người dùng đến nhiều máy chủ backend EC2 thông qua một **Target Group** (`ors-target-group`). ALB cũng thực hiện kiểm tra sức khỏe (health checks) để đảm bảo chỉ gửi request đến các máy chủ đang hoạt động tốt.

*   **Auto Scaling Group (ASG)**: Tự động điều chỉnh số lượng máy chủ EC2 backend dựa trên tải thực tế (ví dụ: số lượng request mỗi phút). Khi lưu lượng tăng, ASG sẽ tự động thêm máy chủ mới và sẽ loại bỏ chúng khi lưu lượng giảm, giúp tối ưu chi phí và đảm bảo hiệu năng.

#### Lưu trữ và Phân phối nội dung (Storage & Content Delivery)

*   **Amazon S3 (Simple Storage Service)**: Sử dụng để lưu trữ toàn bộ mã nguồn frontend (HTML, CSS, JavaScript, hình ảnh) trong một bucket tên `ors-fe`. Bucket này sẽ được cấu hình để hoạt động như một trang web tĩnh.

*   **Amazon CloudFront**: Là dịch vụ mạng phân phối nội dung (CDN) giúp tăng tốc độ và bảo mật cho cả frontend và backend.
    *   **Frontend Distribution**: Phân phối nội dung tĩnh từ S3. Chúng ta sẽ sử dụng **Origin Access Identity (OAI)** để khóa S3 bucket, buộc người dùng phải truy cập trang web thông qua CloudFront, tăng cường bảo mật.
    *   **Backend Distribution**: Cung cấp một endpoint HTTPS an toàn cho API, hoạt động như một proxy và chuyển tiếp các yêu cầu đến **Application Load Balancer**.