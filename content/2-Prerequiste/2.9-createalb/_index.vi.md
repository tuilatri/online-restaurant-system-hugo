---
title : "Tạo Application Load Balancer (ALB)"
date : "2025-09-06"
weight : 9
chapter : false
pre : " <b> 2.9 </b> "
---

### Khởi tạo Application Load Balancer (ALB)

ℹ️ **Mục tiêu**

*   **Application Load Balancer (ALB)** đóng vai trò là điểm tiếp nhận và phân phối lưu lượng truy cập từ người dùng đến các máy chủ backend.
*   Bằng cách đặt ALB trong các **Public Subnet** trên nhiều Availability Zone (AZ), chúng ta đảm bảo ứng dụng có **tính sẵn sàng cao (High Availability)**. Nếu một AZ gặp sự cố, ALB sẽ tự động chuyển hướng traffic đến các máy chủ ở AZ còn lại.
*   ALB sẽ lắng nghe các yêu cầu đến và chuyển tiếp chúng đến **Target Group** (`ors-target-group`) mà chúng ta đã tạo ở bước trước.

---

🔒 **Các bước thực hiện**

#### **1. Bắt đầu tạo Load Balancer**

*   Từ **EC2 Dashboard**, ở menu bên trái, cuộn xuống mục **Load Balancing** và chọn **Load Balancers**.
*   Nhấn vào **Create load balancer**.

{{< figure src="/images/2.prerequisite/2.9-createalb/alb-dashboard.png" title="Bắt đầu tạo Load Balancer" >}}

#### **2. Chọn loại Load Balancer**

*   Vì ứng dụng của chúng ta hoạt động trên lớp ứng dụng (HTTP/HTTPS), hãy chọn **Application Load Balancer** bằng cách nhấn **Create**.

{{< figure src="/images/2.prerequisite/2.9-createalb/alb-choose-type.png" title="Chọn Application Load Balancer" >}}

#### **3. Cấu hình thông tin cơ bản**

*   **Load balancer name:** `ors-alb`
*   **Scheme:** `Internet-facing` (Vì ALB này sẽ nhận traffic trực tiếp từ Internet).
*   **IP address type:** `IPv4`

{{< figure src="/images/2.prerequisite/2.9-createalb/alb-basic-config.png" title="Điền thông tin cấu hình cơ bản cho ALB" >}}

#### **4. Cấu hình Network Mapping**

*   Đây là bước quan trọng để đảm bảo ALB hoạt động trên nhiều AZ và có thể truy cập được từ Internet.
*   **VPC:** Chọn `ors-vpc`.
*   **Mappings:** Chọn cả hai Availability Zone mà chúng ta đang sử dụng. Với mỗi AZ, hãy chọn **Public Subnet** tương ứng (ví dụ: `subnet-public1` cho `ap-southeast-1a` và `subnet-public2` cho `ap-southeast-1b`).

{{< figure src="/images/2.prerequisite/2.9-createalb/alb-network-mapping.png" title="Chọn VPC và các Public Subnet trên cả hai AZ" >}}

#### **5. Cấu hình Security Group**

*   Trong mục **Security groups**, xóa Security Group `default` đang được chọn.
*   Trong danh sách thả xuống, chọn Security Group đã tạo riêng cho Web Server: `ors-sg`.

{{< figure src="/images/2.prerequisite/2.9-createalb/alb-security-group.png" title="Chọn Security Group ors-sg cho ALB" >}}

#### **6. Cấu hình Listeners and Routing**

*   Đây là nơi chúng ta định nghĩa cách ALB xử lý các yêu cầu đến.
*   **Listener:** Giữ nguyên `HTTP` và Port `80`.
*   **Default action:** Trong danh sách thả xuống, chọn `ors-target-group` đã tạo ở bước trước. Thao tác này sẽ ra lệnh cho ALB chuyển tiếp tất cả traffic từ port 80 đến Target Group này.

{{< figure src="/images/2.prerequisite/2.9-createalb/alb-listeners-routing.png" title="Cấu hình Listener và chuyển tiếp đến Target Group" >}}

#### **7. Hoàn tất, kiểm tra trạng thái và truy cập thử**

*   Kiểm tra lại các thông tin trong phần **Summary** và nhấn **Create load balancer**.
*   Sau khi tạo thành công, nhấn **View load balancer**.

{{% notice info %}}
**Lưu ý:** Quá trình khởi tạo Load Balancer sẽ mất khoảng vài phút. Trạng thái của nó sẽ chuyển từ `provisioning` sang `active`. Hãy kiên nhẫn chờ đợi.
{{% /notice %}}

{{< figure src="/images/2.prerequisite/2.9-createalb/alb-status-check.png" title="Chờ ALB chuyển sang trạng thái Active" >}}

*   Khi ALB đã `active`, chọn vào nó và sao chép **DNS name** trong tab **Details**.

{{< figure src="/images/2.prerequisite/2.9-createalb/alb-copy-dns.png" title="Sao chép DNS name của ALB" >}}

*   Dán DNS name này vào trình duyệt của bạn. Nếu mọi thứ được cấu hình chính xác và ứng dụng backend của bạn đang chạy trên instance `ors-ec2`, bạn sẽ thấy thông báo chào mừng từ API: `Welcome to Dining Verse Backend API!`

{{< figure src="/images/2.prerequisite/2.9-createalb/alb-test-result.png" title="Kết quả truy cập thành công qua DNS của ALB" >}}