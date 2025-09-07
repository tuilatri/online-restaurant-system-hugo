---
title : "Tạo EC2 Instance ban đầu"
date : "2025-09-06"
weight : 4
chapter : false
pre : " <b> 2.4 </b> "
---

### Khởi tạo EC2 Instance ban đầu để cấu hình

ℹ️ **Mục tiêu**

*   Khởi tạo một máy chủ ảo (**EC2 Instance**) đầu tiên, đặt tên là `ors-ec2`.
*   Máy chủ này sẽ được đặt trong **Public Subnet** để chúng ta có thể truy cập, cài đặt môi trường (Node.js, Git,...) và cấu hình ứng dụng.
*   Sau khi hoàn tất, instance này sẽ được dùng làm "khuôn mẫu" để tạo **AMI (Amazon Machine Image)** cho việc tự động mở rộng sau này.

---

🔒 **Các bước thực hiện**

#### **1. Bắt đầu khởi tạo Instance**

*   Trong giao diện **AWS Management Console**, tìm và chọn dịch vụ **EC2**.
*   Từ **EC2 Dashboard**, nhấn vào nút **Launch instance**.

{{< figure src="/images/2.prerequisite/2.4-createec2/ec2-dashboard.png" title="Bắt đầu Launch Instance từ EC2 Dashboard" >}}

#### **2. Đặt tên và chọn AMI**

*   **Name:** `ors-ec2`
*   **Application and OS Images (Amazon Machine Image):** Chọn **Amazon Linux**. Đây là hệ điều hành được tối ưu cho AWS và tương thích tốt với các công cụ chúng ta sẽ sử dụng.

{{< figure src="/images/2.prerequisite/2.4-createec2/ec2-name-ami.png" title="Đặt tên và chọn Amazon Machine Image" >}}

#### **3. Chọn loại Instance**

*   **Instance type:** Chọn `t2.micro`. Loại instance này thuộc chương trình **Free Tier** của AWS, phù hợp cho việc học tập và phát triển.

#### **4. Tạo Key Pair để truy cập**

*   Đây là bước **cực kỳ quan trọng** để có thể kết nối vào máy chủ qua SSH.
*   Trong mục **Key pair (login)**, nhấn vào **Create new key pair**.
*   **Key pair name:** `ors-keypair`
*   **Key pair type:** `RSA`
*   **Private key file format:** `.pem` (dùng cho MobaXterm hoặc Terminal trên macOS/Linux).
*   Nhấn **Create key pair** và trình duyệt sẽ tự động tải về file `ors-keypair.pem`.

{{% notice warning %}}
**QUAN TRỌNG:** Hãy lưu trữ file `.pem` này ở một nơi an toàn và không bao giờ chia sẻ nó. Bạn sẽ **không thể tải lại** file này lần thứ hai. Nếu làm mất, bạn sẽ mất quyền truy cập vào EC2 instance.
{{% /notice %}}

{{< figure src="/images/2.prerequisite/2.4-createec2/ec2-create-keypair.png" title="Tạo Key Pair mới để truy cập an toàn" >}}

#### **5. Cấu hình Network Settings**

*   Nhấn vào nút **Edit** ở mục **Network settings**.
*   **VPC:** Chọn `ors-vpc` đã tạo.
*   **Subnet:** Chọn một trong hai **Public Subnet** đã cấu hình (ví dụ: `ors-vpc-subnet-public1-ap-southeast-1a`).
*   **Auto-assign public IP:** `Enable`. (Đây là lý do chúng ta đã cấu hình subnet ở bước 2.2).
*   **Firewall (security groups):** Chọn **Select existing security group**.
*   Trong danh sách **Common security groups**, chọn `ors-sg` mà chúng ta đã tạo ở bước 2.3.

{{< figure src="/images/2.prerequisite/2.4-createec2/ec2-network-settings.png" title="Cấu hình mạng chi tiết cho EC2 instance" >}}

#### **6. Khởi tạo và kiểm tra Instance**

*   Kiểm tra lại các thông tin đã cấu hình trong bảng **Summary** ở bên phải.
*   Nhấn **Launch instance**.
*   Sau khi khởi tạo thành công, bạn có thể nhấn **View all instances** để xem máy chủ `ors-ec2` của mình.

*   Đợi một vài phút cho đến khi cột **Status check** chuyển thành **2/2 checks passed**. Lúc này, máy chủ của bạn đã sẵn sàng để kết nối.

{{< figure src="/images/2.prerequisite/2.4-createec2/ec2-view-instance.png" title="Kiểm tra EC2 instance trong danh sách" >}}