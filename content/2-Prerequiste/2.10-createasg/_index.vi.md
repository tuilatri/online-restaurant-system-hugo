---
title : "Tạo Auto Scaling Group (ASG)"
date : "2025-09-06"
weight : 10
chapter : false
pre : " <b> 2.10 </b> "
---

### Khởi tạo Auto Scaling Group (ASG)

ℹ️ **Mục tiêu**

*   **Auto Scaling Group (ASG)** là trái tim của một kiến trúc linh hoạt trên AWS. Nhiệm vụ của nó là tự động điều chỉnh số lượng EC2 instance để đáp ứng nhu cầu lưu lượng truy cập.
*   Khi traffic tăng, ASG sẽ tự động thêm instance mới (**Scale Out**). Khi traffic giảm, nó sẽ loại bỏ các instance không cần thiết (**Scale In**) để tiết kiệm chi phí.
*   Chúng ta sẽ cấu hình một ASG sử dụng **Launch Template** (`ors-launch-template`) để biết *cách* tạo instance, và gắn nó với **Load Balancer** (thông qua Target Group) để biết *khi nào* cần hành động.

---

🔒 **Các bước thực hiện**

#### **1. Bắt đầu tạo Auto Scaling Group**

*   Từ **EC2 Dashboard**, ở menu bên trái, cuộn xuống dưới cùng và chọn **Auto Scaling Groups**.
*   Nhấn vào **Create Auto Scaling group**.

{{< figure src="/images/2.prerequisite/2.10-createasg/asg-dashboard.png" title="Bắt đầu tạo Auto Scaling Group" >}}

#### **2. Chọn Launch Template và Đặt tên**

*   **Auto Scaling group name:** `ors-asg`
*   **Launch template:** Chọn `ors-launch-template` từ danh sách.
*   Sau khi chọn, nhấn **Next**.

{{< figure src="/images/2.prerequisite/2.10-createasg/asg-name-template.png" title="Đặt tên và chọn Launch Template" >}}

#### **3. Cấu hình Network**

*   **VPC:** Chọn `ors-vpc`.
*   **Availability Zones and subnets:** Chọn cả hai **Public Subnet** mà chúng ta có. ASG sẽ sử dụng các subnet này để khởi chạy instance mới, đảm bảo chúng được phân bổ đều trên hai AZ để tăng tính sẵn sàng.
*   Nhấn **Next**.

{{< figure src="/images/2.prerequisite/2.10-createasg/asg-network-config.png" title="Cấu hình VPC và các Public Subnet" >}}

#### **4. Tích hợp với Load Balancer**

*   Chọn **Attach to an existing load balancer**.
*   Chọn **Choose from your load balancer target groups**.
*   Trong danh sách thả xuống, chọn `ors-target-group`.
*   Nhấn **Next**.

{{< figure src="/images/2.prerequisite/2.10-createasg/asg-load-balancing.png" title="Gắn ASG vào Target Group đã tồn tại" >}}

#### **5. Cấu hình Quy mô và Chính sách Scaling**

*   **Group size (Quy mô nhóm):**
    *   **Desired capacity:** `1` (Số lượng instance mong muốn khi ASG được tạo).
    *   **Minimum capacity:** `1` (Luôn duy trì ít nhất 1 instance).
    *   **Maximum capacity:** `2` (Cho phép tạo tối đa 2 instance).

*   **Scaling policies (Chính sách mở rộng):**
    *   Chọn **Target tracking scaling policy**.
    *   **Scaling policy name:** `Target Tracking Policy`
    *   **Metric type:** `Application Load Balancer request count per target`.
    *   **Target value:** `30`.

{{% notice info %}}
**Chính sách này hoạt động như thế nào?**
Bạn đang ra lệnh cho ASG: "Hãy theo dõi số lượng request trung bình mà mỗi EC2 instance phải xử lý. Nếu con số này **vượt quá 30 request/phút**, hãy tự động tạo thêm một instance mới (tối đa là 2) để san sẻ công việc. Ngược lại, nếu nó thấp hơn 30, hãy giảm bớt instance (tối thiểu là 1) để tiết kiệm tiền."
{{% /notice %}}

{{< figure src="/images/2.prerequisite/2.10-createasg/asg-size-scaling-01.png" >}}
{{< figure src="/images/2.prerequisite/2.10-createasg/asg-size-scaling-02.png" title="Cấu hình quy mô và chính sách Scaling" >}}

*   Nhấn **Next** cho đến khi bạn đến trang **Review**.

#### **6. Review và Hoàn tất**

*   Kiểm tra lại tất cả các thông tin đã cấu hình trên trang **Review**.
*   Cuộn xuống dưới và nhấn **Create Auto Scaling group**.

#### **7. Kiểm tra kết quả**

*   Sau khi tạo xong, ASG sẽ xuất hiện trong danh sách.
*   Chọn vào `ors-asg` và chuyển sang tab **Instance management**.
*   Bạn sẽ thấy ASG đang trong quá trình khởi tạo một instance mới để đạt được **Desired capacity** là `1`. Hãy đợi cho đến khi **Lifecycle** của instance là `InService`. Điều này cho thấy instance đã sẵn sàng hoạt động và đã được đăng ký thành công vào Target Group.

{{< figure src="/images/2.prerequisite/2.10-createasg/asg-instance-management.png" title="Kiểm tra instance do ASG quản lý" >}}