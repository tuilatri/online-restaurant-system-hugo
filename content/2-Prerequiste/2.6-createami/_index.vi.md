---
title : "Tạo Amazon Machine Image (AMI)"
date : "2025-09-06"
weight : 6
chapter : false
pre : " <b> 2.6 </b> "
---

### Tạo AMI từ EC2 Instance đã cấu hình

ℹ️ **Mục tiêu**

*   **Amazon Machine Image (AMI)** là một bản "snapshot" hay một bản sao lưu hoàn chỉnh của một EC2 instance, bao gồm hệ điều hành, các phần mềm đã cài đặt, và toàn bộ cấu hình.
*   Mục đích của việc tạo AMI trong workshop này là để có một **"khuôn mẫu" chuẩn** cho máy chủ web. Khuôn mẫu này sẽ được **Launch Template** và **Auto Scaling Group** sử dụng ở các bước sau để tự động tạo ra các bản sao y hệt của máy chủ khi cần mở rộng quy mô.

---

🔒 **Các bước thực hiện**

#### **1. Chọn EC2 Instance nguồn**

*   Trong **EC2 Dashboard**, điều hướng đến **Instances**.
*   Từ danh sách, chọn instance `ors-ec2` mà bạn đã tạo và sẽ cấu hình ở các bước sau.

{{< figure src="/images/2.prerequisite/2.6-createami/ami-select-ec2.png" title="Chọn EC2 instance ors-ec2 làm nguồn" >}}

#### **2. Bắt đầu quá trình tạo Image**

*   Sau khi đã chọn instance, nhấn vào menu **Actions**.
*   Chọn **Image and templates**.
*   Chọn **Create image**.

{{< figure src="/images/2.prerequisite/2.6-createami/ami-actions-menu.png" title="Bắt đầu tạo Image từ menu Actions" >}}

#### **3. Cấu hình thông tin cho AMI**

*   Tại trang **Create image**, điền các thông tin sau:
    *   **Image name:** `ors-ami`
    *   **Image description:** `ors-ami`
*   Các tùy chọn khác có thể để mặc định. Việc này đảm bảo AMI sẽ giữ lại toàn bộ cấu hình của instance gốc.

{{< figure src="/images/2.prerequisite/2.6-createami/ami-config.png" title="Điền thông tin cấu hình cho AMI" >}}

#### **4. Hoàn tất và kiểm tra trạng thái**

*   Cuộn xuống dưới và nhấn **Create image**.
*   Bạn sẽ nhận được thông báo rằng yêu cầu tạo AMI đã được gửi đi thành công.

{{< figure src="/images/2.prerequisite/2.6-createami/ami-success.png" title="Thông báo yêu cầu tạo AMI thành công" >}}

*   Để theo dõi tiến trình:
    *   Ở menu bên trái, dưới mục **Images**, chọn **AMIs**.
    *   Bạn sẽ thấy AMI `ors-ami` đang ở trạng thái `pending`.
    *   Quá trình này có thể mất vài phút. Hãy đợi cho đến khi cột **Status** chuyển sang `Available`.

{{< figure src="/images/2.prerequisite/2.6-createami/ami-status-check.png" title="Kiểm tra trạng thái AMI đã chuyển sang Available" >}}

{{% notice success %}}
Tuyệt vời! Bạn đã tạo thành công một khuôn mẫu máy chủ hoàn chỉnh. Bất kỳ EC2 instance nào được tạo từ AMI này sẽ có sẵn mọi thứ bạn đã cài đặt, sẵn sàng để phục vụ người dùng ngay lập tức.
{{% /notice %}}