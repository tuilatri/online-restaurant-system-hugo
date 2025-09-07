---
title : "Chỉnh sửa Public Subnet"
date : "2025-09-06" 
weight : 2 
chapter : false
pre : " <b> 2.2 </b> "
---

### Cấu hình Auto-assign Public IP cho Public Subnet

ℹ️ **Mục tiêu**

*   Kích hoạt tính năng tự động gán địa chỉ IP công cộng (**Public IP**) cho các EC2 instance được khởi chạy trong **Public Subnet**.
*   Đây là bước **bắt buộc** để chúng ta có thể truy cập và cấu hình máy chủ ban đầu từ Internet thông qua SSH.

---

🔒 **Các bước thực hiện**

#### **1. Truy cập trang quản lý Subnet**

*   Từ giao diện **VPC Dashboard**, chọn **Subnets** ở menu bên trái để xem danh sách tất cả các subnet thuộc VPC `ors-vpc` của bạn.

{{< figure src="/images/2.prerequisite/2.2-modifysubnet/subnet-list-01.png" title="Danh sách các Subnet của vpc-ors" >}}

#### **2. Chỉnh sửa Public Subnet đầu tiên**

*   Xác định và chọn một trong hai **Public Subnet** đã tạo (ví dụ: `ors-vpc-subnet-public1-ap-southeast-1a`).
*   Nhấn vào nút **Actions** và chọn **Edit subnet settings**.

{{< figure src="/images/2.prerequisite/2.2-modifysubnet/subnet-action-01-01.png" title="Chọn Edit subnet settings" >}}

#### **3. Kích hoạt tính năng Auto-assign IP**

*   Trong trang **Edit subnet settings**, tìm đến mục **Auto-assign IP settings**.
*   Tích vào ô **Enable auto-assign public IPv4 address**.
*   Cuộn xuống dưới và nhấn **Save** để lưu lại thay đổi.

{{< figure src="/images/2.prerequisite/2.2-modifysubnet/subnet-edit-01-01.png" title="Kích hoạt tính năng Auto-assign Public IP" >}}

{{% notice success %}}
Khi tính năng này được bật, bất kỳ EC2 instance nào được khởi chạy trong subnet này sẽ tự động nhận được một địa chỉ IP công cộng, giúp nó có thể giao tiếp với Internet.
{{% /notice %}}

#### **4. Lặp lại cho Public Subnet còn lại**

*   Thực hiện lại các bước **2** và **3** cho **Public Subnet thứ hai** (ví dụ: `ors-vpc-subnet-public2-ap-southeast-1b`).
*   Việc này đảm bảo rằng hệ thống của chúng ta có thể hoạt động đồng đều trên cả hai Availability Zone, duy trì tính sẵn sàng cao.

{{< figure src="/images/2.prerequisite/2.2-modifysubnet/subnet-action-02-01.png" >}}
{{< figure src="/images/2.prerequisite/2.2-modifysubnet/subnet-edit-02-01.png" title="Hoàn tất cấu hình cho cả hai Public Subnet" >}}