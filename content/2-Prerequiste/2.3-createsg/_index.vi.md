---
title : "Tạo Security Groups"
date : "2025-09-06" 
weight : 3 
chapter : false
pre : " <b> 2.3 </b> "
---

### Tạo các Lớp Bảo mật (Security Group)

ℹ️ **Mục tiêu**

*   **Security Group** hoạt động như một bức tường lửa ảo ở cấp độ instance để kiểm soát lưu lượng truy cập ra vào.
*   Trong phần này, chúng ta sẽ tạo ra 2 Security Group riêng biệt cho hai thành phần cốt lõi trong kiến trúc:
    1.  **Web Server (`ors-sg`):** Nhận lưu lượng truy cập từ người dùng và cho phép quản trị viên kết nối vào.
    2.  **Database (`ors-db-sg`):** Chỉ cho phép kết nối từ Web Server và quản trị viên, đảm bảo an toàn tuyệt đối cho dữ liệu.

---

🔒 **Các bước thực hiện**

#### **1. Tạo Security Group cho Web Server (`ors-sg`)**

Đây là lớp bảo vệ cho các máy chủ EC2 sẽ chạy ứng dụng backend Node.js.

*   **Bước 1: Bắt đầu tạo Security Group**
    *   Từ giao diện **VPC Dashboard**, chọn **Security Groups** ở menu bên trái.
    *   Nhấn vào **Create security group**.

    {{< figure src="/images/2.prerequisite/2.3-createsg/sg-create.png" title="Tạo Security Group mới" >}}

*   **Bước 2: Cấu hình thông tin cơ bản**
    *   **Security group name:** `ors-sg`
    *   **Description:** `Security group for Online Restaurant Web Server`
    *   **VPC:** Chọn VPC `ors-vpc` đã tạo ở bước trước.

    {{< figure src="/images/2.prerequisite/2.3-createsg/sg-webserver-config.png"  title="Thông tin cơ bản của Web Server SG" >}}

*   **Bước 3: Thiết lập Inbound Rules (Luồng truy cập vào)**
    *   Trong mục **Inbound rules**, nhấn **Add rule** và cấu hình 4 quy tắc sau:

| Type | Protocol | Port range | Source | Description |
| :--- | :--- | :--- | :--- | :--- |
| SSH | TCP | 22 | My IP | Cho phép **bạn** kết nối SSH từ IP hiện tại để quản lý máy chủ. |
| HTTP | TCP | 80 | Anywhere-IPv4 | Cho phép người dùng truy cập web server qua HTTP. |
| HTTPS | TCP | 443 | Anywhere-IPv4 | Cho phép người dùng truy cập an toàn qua HTTPS. |
| Custom TCP | TCP | 5000 | Anywhere-IPv4 | Mở cổng cho ứng dụng Node.js (dùng cho kiểm tra và cân bằng tải). |

    {{< figure src="/images/2.prerequisite/2.3-createsg/sg-webserver-inbound.png" title="Cấu hình Inbound Rules cho Web Server SG" >}}

{{% notice info %}}
Khi chọn **Source** là **My IP**, AWS sẽ tự động điền địa chỉ IP công cộng của bạn. Điều này giúp tăng cường bảo mật bằng cách chỉ cho phép quản trị viên truy cập từ một địa điểm tin cậy.
{{% /notice %}}

*   **Bước 4:** Cuộn xuống dưới cùng và nhấn **Create security group**.

---

#### **2. Tạo Security Group cho Database (`ors-db-sg`)**

Lớp bảo vệ này được thiết kế để "khóa" cơ sở dữ liệu, chỉ cho phép các kết nối thực sự cần thiết.

*   **Bước 1: Cấu hình thông tin cơ bản**
    *   Nhấn **Create security group** một lần nữa.
    *   **Security group name:** `ors-db-sg`
    *   **Description:** `Security group for Online Restaurant Database`
    *   **VPC:** Chọn VPC `ors-vpc`.

    {{< figure src="/images/2.prerequisite/2.3-createsg/sg-db-config.png" title="Thông tin cơ bản của Database SG" >}}

*   **Bước 2: Thiết lập Inbound Rules**
    *   Nhấn **Add rule** và cấu hình 2 quy tắc sau:

| Type | Protocol | Port range | Source | Description |
| :--- | :--- | :--- | :--- | :--- |
| MYSQL/Aurora | TCP | 3306 | Custom -> `ors-sg` | **Quan trọng:** Chỉ cho phép các máy chủ web (thuộc `ors-sg`) kết nối tới DB. |
| MYSQL/Aurora | TCP | 3306 | My IP | Cho phép **bạn** kết nối vào DB từ máy cá nhân để quản lý (ví dụ: dùng MySQL Workbench). |

{{% notice success %}}
**Nguyên tắc bảo mật quan trọng:** Bằng cách chọn một Security Group khác (`ors-sg`) làm **Source**, chúng ta tạo ra một quy tắc động. Bất kỳ máy chủ nào thuộc `ors-sg` đều có thể kết nối tới cơ sở dữ liệu mà không cần phải chỉ định địa chỉ IP cụ thể. Đây là cách làm tốt nhất để bảo mật trong môi trường đám mây.
{{% /notice %}}

    {{< figure src="/images/2.prerequisite/2.3-createsg/sg-db-inbound.png" title="Cấu hình Inbound Rules cho Database SG" >}}

*   **Bước 3:** Nhấn **Create security group**.

Sau khi hoàn thành, bạn sẽ có 2 Security Group đã được cấu hình chính xác và sẵn sàng để bảo vệ các tài nguyên của dự án.