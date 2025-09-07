---
title : "Tạo VPC và Subnets"
date : "2025-09-06" 
weight : 1 
chapter : false
pre : " <b> 2.1 </b> "
---

### Tạo Môi trường Mạng (VPC)

ℹ️ **Mục tiêu**

*   Tạo một môi trường mạng ảo (**VPC**) riêng biệt trong AWS cho dự án nhà hàng.
*   Sử dụng trình hướng dẫn **"VPC and more"** để tự động thiết lập các thành phần mạng cần thiết, bao gồm **Public Subnets**, **Private Subnets**, **Internet Gateway**, và **Route Tables**.

---

🔒 **Các bước thực hiện**

#### **1. Truy cập dịch vụ VPC**

*   Từ giao diện **AWS Management Console**, tìm kiếm dịch vụ **VPC**.
*   Chọn **VPC** từ kết quả tìm kiếm để truy cập vào trang quản lý VPC.

{{< figure src="/images/2.prerequisite/2.1-createvpc/vpc-search.png" title="Tìm kiếm dịch vụ VPC" >}}

#### **2. Bắt đầu tạo VPC**

*   Trong giao diện VPC Dashboard, ở menu bên trái, chọn **Your VPCs**.
*   Nhấn vào nút **Create VPC** ở góc trên bên phải.

{{< figure src="/images/2.prerequisite/2.1-createvpc/vpc-choose.png" title="Nhấn nút Create VPC" >}}

#### **3. Cấu hình thông số VPC**

*   Tại trang **Create VPC**, trong phần **Resources to create**, hãy chắc chắn rằng bạn đã chọn **VPC and more** để sử dụng trình hướng dẫn tạo tự động.

*   **Cấu hình chi tiết:**
    *   **Name tag auto-generation:** `ors-vpc`
    *   **IPv4 CIDR block:** Để mặc định `10.0.0.0/16`.
    *   **Number of Availability Zones (AZs):** `2`
    *   **Number of Public subnets:** `2`
    *   **Number of Private subnets:** `2`
    *   **NAT gateways:** `None` (Để tiết kiệm chi phí cho workshop này).
    *   **VPC endpoints:** `None`

{{< figure src="/images/2.prerequisite/2.1-createvpc/vpc-edit-01.png" >}}
{{< figure src="/images/2.prerequisite/2.1-createvpc/vpc-edit-02.png" title="Cấu hình VPC and more cho dự án" >}}

{{% notice info %}}
**Giải thích:** Chúng ta tạo ra 2 Public Subnet để đặt các tài nguyên cần truy cập Internet như Load Balancer, và 2 Private Subnet để bảo vệ các tài nguyên nhạy cảm như EC2 backend và cơ sở dữ liệu RDS. Việc trải rộng trên 2 AZ giúp tăng tính sẵn sàng cao (High Availability) cho hệ thống.
{{% /notice %}}

#### **4. Hoàn tất và tạo VPC**

*   Sau khi đã điền đầy đủ thông tin, cuộn xuống dưới và nhấn vào **Create VPC**.

{{< figure src="/images/2.prerequisite/2.1-createvpc/vpc-edit-03.png" title="Hoàn tất và tạo VPC" >}}

#### **5. Kiểm tra kết quả**

*   Quá trình tạo các tài nguyên mạng có thể mất vài phút. Sau khi hoàn tất, bạn sẽ thấy màn hình thông báo thành công.
*   Nhấn vào **View VPC** để xem lại các tài nguyên đã được tạo.

{{< figure src="/images/2.prerequisite/2.1-createvpc/vpc-done.png" title="Thông báo tạo VPC thành công" >}}

*   Bạn sẽ được chuyển đến trang danh sách các VPC. Tại đây, VPC `ors-vpc` vừa tạo đã ở trạng thái **Available**, sẵn sàng cho các bước cấu hình tiếp theo.

{{< figure src="/images/2.prerequisite/2.1-createvpc/vpc-check-01.png" title="Kiểm tra VPC trong danh sách" >}}