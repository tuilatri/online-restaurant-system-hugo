---
title : "Các bước chuẩn bị và dựng hạ tầng"
date : "2025-09-06"
weight : 2
chapter : false
pre : " <b> 2. </b> "
---

Trang này cung cấp tổng quan về các bước cần thiết để thiết lập hạ tầng hoàn chỉnh cho dự án nhà hàng trên AWS. Mỗi phần trong danh sách dưới đây sẽ dẫn bạn đến một trang hướng dẫn chi tiết.

{{% notice info %}}
Hãy thực hiện tuần tự các bước dưới đây. Việc thiết lập sai thứ tự có thể dẫn đến lỗi cấu hình và các thành phần không thể giao tiếp với nhau.
{{% /notice %}}

### Nội dung chính của chương

1.  **Cấu hình Môi trường Mạng (VPC)**
    *   [Tạo VPC và Subnets](2.1-createvpc/)
    *   [Chỉnh sửa Public Subnet](2.2-modifysubnet/)

2.  **Thiết lập các Lớp Bảo mật (Security Group)**
    *   [Tạo Security Groups cho Web Server và Database](2.3-createsg/)

3.  **Khởi tạo Máy chủ và Cơ sở dữ liệu**
    *   [Tạo EC2 Instance ban đầu để cấu hình](2.4-createec2/)
    *   [Tạo Cơ sở dữ liệu RDS MySQL](2.5-createrds/)
    *(Lưu ý: Bước tạo RDS đã được thêm vào đây để đảm bảo luồng workshop liền mạch)*

4.  **Chuẩn bị cho việc Tự động Mở rộng (Automation)**
    *   [Tạo Amazon Machine Image (AMI) từ EC2 đã cấu hình](2.6-createami/)
    *   [Tạo Launch Template cho Auto Scaling Group](2.7-createlaunchtemplate/)

5.  **Thiết lập Cân bằng tải (Load Balancer)**
    *   [Tạo Target Group](2.8-createtargetgroup/)
    *   [Tạo Application Load Balancer (ALB)](2.9-createalb/)

6.  **Cấu hình Tự động Mở rộng (Auto Scaling)**
    *   [Tạo Auto Scaling Group (ASG)](2.10-createasg/)

7.  **Lưu trữ và Phân phối Nội dung (Storage & CDN)**
    *   [Tạo S3 Bucket cho Frontend](2.11-creates3/)
    *   [Tạo CloudFront cho Backend (trỏ tới ALB)](2.12-createcloudfrontbe/)
    *   [Tạo CloudFront cho Frontend (trỏ tới S3)](2.13-createcloudfrontfe/)