---
title : "Tạo Cơ sở dữ liệu RDS MySQL"
date : "2025-09-06"
weight : 5
chapter : false
pre : " <b> 2.5 </b> "
---

### Khởi tạo Cơ sở dữ liệu với Amazon RDS

ℹ️ **Mục tiêu**

*   Tạo một cơ sở dữ liệu **MySQL** được quản lý bằng dịch vụ **Amazon RDS**.
*   Đặt cơ sở dữ liệu này vào trong môi trường mạng **VPC (`ors-vpc`)** đã tạo và bảo vệ nó bằng **Security Group (`ors-db-sg`)**.
*   Sử dụng **Free Tier** để tiết kiệm chi phí trong quá trình thực hành workshop.

---

🔒 **Các bước thực hiện**

#### **1. Truy cập dịch vụ RDS**

*   Từ giao diện **AWS Management Console**, tìm kiếm và chọn dịch vụ **RDS**.

{{< figure src="/images/2.prerequisite/2.5-createrds/rds-search.png" title="Tìm kiếm dịch vụ RDS" >}}

#### **2. Bắt đầu tạo Database**

*   Trong **RDS Dashboard**, nhấn vào nút **Create database**.

{{< figure src="/images/2.prerequisite/2.5-createrds/rds-dashboard.png" title="Nhấn Create database" >}}

#### **3. Cấu hình Engine và Template**

*   **Choose a database creation method:** Chọn **Standard create**.
*   **Engine options:** Chọn **MySQL**.
*   **Templates:** Chọn **Production**.

{{% notice info %}}
Việc chọn **Free tier** sẽ tự động giới hạn các tùy chọn cấu hình (ví dụ: chỉ có thể chọn Single DB instance) để đảm bảo bạn không phát sinh chi phí ngoài ý muốn.
{{% /notice %}}

{{< figure src="/images/2.prerequisite/2.5-createrds/rds-engine-template.png" title="Chọn Standard create, MySQL và Free Tier" >}}

#### **4. Cấu hình Settings**

*   **DB instance identifier:** `ors-db`
*   **Master username:** `admin`
*   **Master password:** Nhập mật khẩu của bạn (ví dụ: `0812-haminhtri`).
*   **Confirm password:** Nhập lại mật khẩu.

{{< figure src="/images/2.prerequisite/2.5-createrds/rds-settings.png" title="Cấu hình thông tin đăng nhập cho Database" >}}

#### **5. Cấu hình Connectivity (Kết nối)**

Đây là bước quan trọng để đặt cơ sở dữ liệu vào đúng môi trường mạng.

*   **Virtual private cloud (VPC):** Chọn `ors-vpc`.
*   **DB Subnet Group:** Để mặc định, AWS sẽ tự động tạo một Subnet Group mới phù hợp với VPC của bạn.
*   **Public access:** Chọn **Yes**.
*   **VPC security group (firewall):** Chọn **Choose existing**.
*   **Existing VPC security groups:** Giữ Security Group `default` và chọn `ors-db-sg`.

<!-- {{% notice warning %}}
**Lý do chọn Public access = Yes:**
Trong workshop này, chúng ta cần kết nối tới database từ máy tính cá nhân (sử dụng MySQL Workbench) để import dữ liệu ban đầu. Việc này được bảo mật bởi **Security Group `ors-db-sg`**, vốn chỉ cho phép truy cập từ IP của bạn (quy tắc **My IP** đã tạo ở bước 2.3). Trong môi trường Production thực tế, bạn nên chọn **No** và truy cập thông qua một Bastion Host.
{{% /notice %}} -->

{{< figure src="/images/2.prerequisite/2.5-createrds/rds-connectivity.png" title="Cấu hình kết nối mạng cho RDS" >}}

#### **6. Cấu hình Additional Configuration**

*   Mở rộng mục **Additional configuration**.
*   **Initial database name:** `ors`
*   Đây chính là tên schema (cơ sở dữ liệu) ban đầu mà ứng dụng sẽ sử dụng.

{{< figure src="/images/2.prerequisite/2.5-createrds/rds-additional-config.png" title="Đặt tên cho schema ban đầu" >}}

#### **7. Hoàn tất, tạo và kiểm tra**

*   Cuộn xuống dưới cùng và nhấn **Create database**.
*   Quá trình khởi tạo database có thể mất từ 5 đến 10 phút. Hãy đợi cho đến khi cột **Status** chuyển sang **Available**.

{{< figure src="/images/2.prerequisite/2.5-createrds/rds-creating.png" title="Đợi Database được tạo" >}}

*   Sau khi hoàn tất, nhấn vào instance `ors-db` vừa tạo.
*   Trong tab **Connectivity & security**, tìm và sao chép lại giá trị **Endpoint**. Đây chính là địa chỉ để kết nối tới cơ sở dữ liệu của bạn.

{{< figure src="/images/2.prerequisite/2.5-createrds/rds-endpoint.png" title="Lấy thông tin Endpoint để kết nối" >}}