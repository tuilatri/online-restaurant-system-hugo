---
title : "Triển khai dự án Nhà hàng Online trên AWS"
date : "2025-09-06" 
weight : 1 
chapter : false
---
# Triển khai dự án Nhà hàng Online Full-Stack trên AWS

### Tổng quan

Trong workshop này, bạn sẽ được hướng dẫn từng bước để triển khai một ứng dụng nhà hàng online hoàn chỉnh lên nền tảng Amazon Web Services (AWS). Dự án bao gồm một frontend được xây dựng bằng HTML, CSS, và JavaScript, cùng với một backend mạnh mẽ sử dụng Node.js (Express) và cơ sở dữ liệu MySQL (RDS), tạo nên một hệ thống có khả năng mở rộng, an toàn và hiệu suất cao.

![Kiến trúc dự án trên AWS](/images/ors-demo.png) 

### Công nghệ và Dịch vụ sử dụng

ℹ️ Mục tiêu của workshop là xây dựng một hạ tầng đám mây chuyên nghiệp, có khả năng chịu lỗi và tự động co giãn để đáp ứng nhu cầu truy cập của người dùng.

💡 **Các dịch vụ AWS chính được sử dụng:**

*   **VPC (Virtual Private Cloud):** Tạo một môi trường mạng riêng biệt và an toàn trên AWS, bao gồm Public Subnet cho các tài nguyên cần truy cập internet và Private Subnet để bảo vệ cơ sở dữ liệu.
*   **EC2 (Elastic Compute Cloud):** Nơi triển khai và chạy ứng dụng backend Node.js.
*   **AMI & Launch Templates:** Tạo một "khuôn mẫu" cho máy chủ, giúp tự động hóa việc khởi tạo các EC2 instance mới một cách đồng nhất.
*   **Application Load Balancer (ALB):** Phân phối lưu lượng truy cập đến nhiều EC2 instance, tăng tính sẵn sàng và khả năng chịu tải của ứng dụng.
*   **Auto Scaling Group (ASG):** Tự động điều chỉnh số lượng EC2 instance dựa trên lưu lượng truy cập, đảm bảo hiệu suất ổn định và tối ưu chi phí.
*   **RDS (Relational Database Service):** Cung cấp một cơ sở dữ liệu MySQL được quản lý hoàn toàn, giúp đơn giản hóa việc cài đặt, vận hành và sao lưu.
*   **S3 (Simple Storage Service):** Lưu trữ và phân phát các tài nguyên tĩnh của frontend (HTML, CSS, JavaScript, hình ảnh).
*   **CloudFront:** Dịch vụ mạng phân phối nội dung (CDN) giúp tăng tốc độ tải trang cho người dùng trên toàn cầu và bảo mật cho cả frontend và backend.

### Kiến trúc và Phạm vi

ℹ️ Dự án được phân tách rõ ràng thành hai phần: **Frontend** (giao diện người dùng) và **Backend** (hệ thống xử lý), được triển khai độc lập nhưng giao tiếp chặt chẽ với nhau qua API.

**Backend (Phía máy chủ)**

*   Ứng dụng Node.js được triển khai trên các **EC2 instance** nằm trong một **Auto Scaling Group**.
*   **Application Load Balancer** sẽ nhận các yêu cầu từ người dùng và chuyển tiếp đến các EC2 instance đang hoạt động.
*   Cơ sở dữ liệu **RDS MySQL** được đặt trong Private Subnet, chỉ cho phép các EC2 instance truy cập để đảm bảo an toàn tối đa.
*   Một **CloudFront distribution** được cấu hình để trỏ đến ALB, cung cấp một lớp bảo vệ và tăng tốc cho các API endpoint.

🔒 **Frontend (Phía người dùng)**

*   Toàn bộ mã nguồn frontend (các file tĩnh) được tải lên một **S3 bucket**.
*   S3 bucket này được cấu hình để hoạt động như một trang web tĩnh (Static website hosting).
*   Một **CloudFront distribution** khác được thiết lập để phục vụ nội dung từ S3 bucket. Việc sử dụng **Origin Access Identity (OAI)** đảm bảo người dùng chỉ có thể truy cập frontend thông qua CloudFront, không thể truy cập trực tiếp vào S3 bucket.

💡 **Hãy cùng bắt đầu xây dựng một hạ tầng mạnh mẽ và chuyên nghiệp cho ứng dụng của bạn trong các phần tiếp theo!**