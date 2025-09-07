---
title : "Dọn dẹp tài nguyên"
date : "2025-09-04" 
weight : 4
chapter : false
pre : " <b> 4. </b> "
---

{{% notice warning %}}
**CỰC KỲ QUAN TRỌNG:** Đây là bước bắt buộc sau khi bạn hoàn thành workshop. Việc không dọn dẹp tài nguyên sẽ khiến tài khoản AWS của bạn tiếp tục phát sinh chi phí, đặc biệt là với các dịch vụ như NAT Gateway, Application Load Balancer và EC2.
{{% /notice %}}

Để đảm bảo không bỏ sót, chúng ta phải xóa các tài nguyên theo thứ tự ngược lại với lúc tạo ra chúng.

---

🔒 **Các bước thực hiện**

#### **1. Xóa CloudFront Distributions**
Bạn cần xóa cả 2 distribution đã tạo cho Frontend (S3) và Backend (ALB).

*   Truy cập dịch vụ **CloudFront**.
*   Chọn một distribution, nhấn **Disable** và xác nhận.
*   Sau khi distribution đã được disable, chọn lại nó và nhấn **Delete**.
*   Lặp lại quá trình cho distribution còn lại.

#### **2. Xóa Auto Scaling Group**
Thao tác này sẽ tự động xóa luôn EC2 instance do ASG quản lý.

*   Truy cập dịch vụ **EC2** -> **Auto Scaling Groups**.
*   Chọn `project-backend-asg` và nhấn **Delete**.
*   Nhập `delete` để xác nhận và hoàn tất.

#### **3. Xóa Application Load Balancer**
*   Truy cập **EC2** -> **Load Balancers**.
*   Chọn `project-backend-alb`, vào **Actions** và chọn **Delete load balancer**.
*   Nhập `confirm` để xác nhận.

#### **4. Xóa Target Group**
*   Truy cập **EC2** -> **Target Groups**.
*   Chọn `project-backend-target-group`, vào **Actions** và chọn **Delete**.

#### **5. Chấm dứt (Terminate) các EC2 Instance còn lại**
Cần xóa thủ công các instance không thuộc ASG.

*   Truy cập **EC2** -> **Instances**.
*   Chọn các instance `project-bastion-host-ec2` và `project-backend-ec2` (nếu nó chưa bị ASG xóa).
*   Chọn **Instance state** -> **Terminate instance**.

#### **6. Xóa Launch Template**
*   Truy cập **EC2** -> **Launch Templates**.
*   Chọn `project-backend-lauch-template`, vào **Actions** và chọn **Delete template**.

#### **7. Xóa AMI và Snapshot liên quan**
*   **Bước 1: Hủy đăng ký AMI**
    *   Truy cập **EC2** -> **AMIs**.
    *   Chọn `project-backend-ec2-ami`, vào **Actions** và chọn **Deregister AMI**.
*   **Bước 2: Xóa Snapshot**
    *   Truy cập **EC2** -> **Snapshots**.
    *   Tìm Snapshot được tạo bởi AMI (thường có mô tả chứa ID của AMI), chọn nó và nhấn **Actions** -> **Delete snapshot**.

#### **8. Xóa S3 Bucket**
Bạn phải xóa hết các đối tượng bên trong bucket trước.

*   Truy cập dịch vụ **S3**.
*   Vào bucket `project-frontend-030925`.
*   Chọn tất cả các file và nhấn **Delete**.
*   Sau khi bucket đã trống, quay ra ngoài, chọn bucket và nhấn **Delete**.

#### **9. Xóa VPC**
Đây là bước cuối cùng, sẽ xóa VPC và các tài nguyên con như Subnet, Route Table, Internet Gateway, và quan trọng nhất là **NAT Gateway**.

*   Truy cập dịch vụ **VPC**.
*   Chọn **Your VPCs**, chọn `project-vpc`.
*   Nhấn **Actions** -> **Delete VPC**.
*   Một cửa sổ sẽ hiện ra liệt kê các tài nguyên sẽ bị xóa. Nhập `delete` và xác nhận.

#### **10. Xóa Key Pair**
*   Truy cập **EC2** -> **Key Pairs**.
*   Chọn `project-keypair`, vào **Actions** và chọn **Delete**.