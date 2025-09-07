---
title : "Tạo Target Group"
date : "2025-09-06"
weight : 8
chapter : false
pre : " <b> 2.8 </b> "
---

### Khởi tạo Target Group

ℹ️ **Mục tiêu**

*   **Target Group** (Nhóm mục tiêu) được sử dụng để gom nhóm các EC2 instance mà **Application Load Balancer (ALB)** sẽ chuyển hướng lưu lượng truy cập đến.
*   ALB cũng sử dụng Target Group để thực hiện **Health Check** (Kiểm tra sức khỏe), đảm bảo rằng nó chỉ gửi yêu cầu đến các instance đang hoạt động bình thường.
*   Chúng ta sẽ tạo một Target Group để quản lý tất cả các máy chủ web của dự án nhà hàng.

---

🔒 **Các bước thực hiện**

#### **1. Truy cập Target Groups**

*   Từ **EC2 Dashboard**, ở menu bên trái, cuộn xuống mục **Load Balancing** và chọn **Target Groups**.
*   Nhấn vào **Create target group**.

{{< figure src="/images/2.prerequisite/2.8-createtargetgroup/tg-dashboard.png" title="Bắt đầu tạo Target Group" >}}

#### **2. Chọn loại Target**

*   Trong bước **Choose a target type**, chọn **Instances**, vì chúng ta muốn Load Balancer chuyển hướng traffic đến các EC2 instance.
*   Nhấn **Next**.

{{< figure src="/images/2.prerequisite/2.8-createtargetgroup/tg-choose-type.png" title="Chọn Target Type là Instances" >}}

#### **3. Cấu hình chi tiết cho Target Group**

*   **Target group name:** `ors-target-group`
*   **Protocol - Port:** Chọn `HTTP` và nhập `5000`. Đây là port mà ứng dụng Node.js backend của chúng ta sẽ lắng nghe.
*   **VPC:** Chọn `ors-vpc`.
*   **Health checks (Kiểm tra sức khỏe):**
    *   **Health check protocol:** `HTTP`
    *   **Health check path:** `/health`.

{{% notice info %}}
**Health Check Path là gì?**
Đây là một đường dẫn (endpoint) đặc biệt mà bạn cần tạo trong code backend của mình. Application Load Balancer sẽ liên tục gửi request đến đường dẫn `/health` này. Nếu nhận được phản hồi thành công (HTTP 200 OK), nó sẽ coi instance đó là "khỏe mạnh" và tiếp tục gửi traffic người dùng đến. Nếu không, nó sẽ tạm ngưng gửi traffic để chờ instance phục hồi.
{{% /notice %}}


{{< figure src="/images/2.prerequisite/2.8-createtargetgroup/tg-config-01.png" >}}
{{< figure src="/images/2.prerequisite/2.8-createtargetgroup/tg-config-02.png" title="Điền thông tin cấu hình cho Target Group" >}}

#### **4. Đăng ký Target ban đầu**

*   Ở bước này, chúng ta sẽ đăng ký instance `ors-ec2` đã tạo thủ công vào Target Group.
*   Trong bảng **Available instances**, chọn `ors-ec2`.
*   Nhấn vào **Include as pending below**. Thao tác này sẽ đưa instance vào danh sách chờ để được đăng ký vào group.

{{% notice success %}}
**Tại sao lại đăng ký instance ban đầu?**
Việc này giúp chúng ta kiểm tra xem Load Balancer và Target Group có hoạt động chính xác hay không ngay sau khi cấu hình. Các instance được tạo sau này sẽ do **Auto Scaling Group** tự động đăng ký vào Target Group này.
{{% /notice %}}

{{< figure src="/images/2.prerequisite/2.8-createtargetgroup/tg-register-target.png" title="Đăng ký instance ors-ec2 vào Target Group" >}}

#### **5. Hoàn tất và xem lại**

*   Cuộn xuống dưới và nhấn **Create target group**.
*   Sau khi tạo thành công, bạn sẽ được đưa đến trang chi tiết của Target Group. Ban đầu, trạng thái **Health status** của instance có thể là `initial` hoặc `unhealthy`.
*   Hãy đợi một vài phút để Load Balancer thực hiện health check, trạng thái sẽ chuyển sang `healthy`.

{{< figure src="/images/2.prerequisite/2.8-createtargetgroup/tg-success.png" title="Tạo Target Group thành công và kiểm tra trạng thái" >}}