---
title : "Tạo Launch Template"
date : "2025-09-06"
weight : 7
chapter : false
pre : " <b> 2.7 </b> "
---

### Khởi tạo Launch Template

ℹ️ **Mục tiêu**

*   **Launch Template** hoạt động như một "bản thiết kế" chi tiết cho việc khởi tạo EC2 instance. Nó lưu trữ tất cả các thông số cấu hình cần thiết như AMI, loại instance, key pair, và security group.
*   Mục đích của chúng ta là tạo một Launch Template sử dụng **AMI** của máy chủ web (`ors-ami`) đã tạo ở bước trước. **Auto Scaling Group** sẽ dựa vào bản thiết kế này để tự động khởi chạy các instance mới một cách đồng nhất và chính xác.

---

🔒 **Các bước thực hiện**

#### **1. Truy cập Launch Templates**

*   Từ **EC2 Dashboard**, ở menu bên trái, cuộn xuống mục **Instances** và chọn **Launch Templates**.
*   Nhấn vào **Create launch template**.

{{< figure src="/images/2.prerequisite/2.7-createlaunchtemplate/lt-dashboard.png" title="Bắt đầu tạo Launch Template" >}}

#### **2. Cấu hình thông tin cơ bản**

*   **Launch template name:** `ors-launch-template`
*   **Template version description:** `Launch template for Online Restaurant System servers`

{{< figure src="/images/2.prerequisite/2.7-createlaunchtemplate/lt-basic-info.png" title="Điền thông tin cơ bản cho Launch Template" >}}

#### **3. Chọn AMI (Amazon Machine Image)**

*   Trong mục **Application and OS Images (Amazon Machine Image)**, chọn tab **My AMIs**.
*   Chọn **Owned by me** và bạn sẽ thấy AMI `ors-ami` đã tạo ở bước trước. Hãy chọn nó.

{{< figure src="/images/2.prerequisite/2.7-createlaunchtemplate/lt-select-ami.png" title="Chọn AMI ors-ami đã tạo" >}}

#### **4. Chọn Instance Type và Key Pair**

*   **Instance type:** Chọn `t2.micro`.
*   **Key pair (login):** Chọn `ors-keypair` từ danh sách thả xuống.

{{< figure src="/images/2.prerequisite/2.7-createlaunchtemplate/lt-instance-type-keypair.png" title="Chọn loại Instance và Key Pair" >}}

#### **5. Cấu hình Network Settings**

*   Trong mục **Network settings**, chúng ta **không** cần chọn Subnet, vì Auto Scaling Group sẽ tự quyết định điều này.
*   Tuy nhiên, chúng ta cần chỉ định **Security Group**.
*   **Security groups:** Chọn **Select existing security group**.
*   Trong danh sách **Common security groups**, chọn `ors-sg`.

{{< figure src="/images/2.prerequisite/2.7-createlaunchtemplate/lt-network-settings.png" title="Cấu hình Security Group cho Launch Template" >}}

#### **6. Hoàn tất và xem lại Launch Template**

*   Kiểm tra lại tất cả các thông tin đã cấu hình trong bảng **Summary** bên phải.
*   Cuộn xuống dưới và nhấn **Create launch template**.

{{< figure src="/images/2.prerequisite/2.7-createlaunchtemplate/lt-create.png" title="Hoàn tất việc tạo Launch Template" >}}

*   Sau khi tạo thành công, nhấn **View launch templates** để xem lại "bản thiết kế" bạn vừa tạo trong danh sách.

{{< figure src="/images/2.prerequisite/2.7-createlaunchtemplate/lt-view-list.png" title="Xem Launch Template trong danh sách" >}}

{{% notice success %}}
Bây giờ bạn đã có một bản thiết kế hoàn chỉnh. Bất cứ khi nào hệ thống cần một máy chủ web mới, nó chỉ cần sử dụng template này để tạo ra một bản sao hoàn hảo.
{{% /notice %}}