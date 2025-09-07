---
title : "Tạo CloudFront cho Backend (ALB)"
date : "2025-09-06"
weight : 12
chapter : false
pre : " <b> 2.12 </b> "
---

### Tạo Distribution CloudFront cho Backend

ℹ️ **Mục tiêu**

*   **Amazon CloudFront** là một dịch vụ mạng phân phối nội dung (CDN) giúp tăng tốc độ và bảo mật cho ứng dụng web.
*   Trong bước này, chúng ta sẽ tạo một CloudFront distribution để làm "cổng vào" công khai cho backend API, thay vì để người dùng truy cập trực tiếp vào ALB.
*   **Lợi ích chính:**
    *   **HTTPS:** CloudFront sẽ cung cấp một endpoint HTTPS an toàn cho người dùng. Nó sẽ giao tiếp với ALB qua HTTP trong mạng nội bộ của AWS, giúp đơn giản hóa việc quản lý chứng chỉ SSL.
    *   **Hiệu suất:** CloudFront đưa API của bạn đến gần người dùng hơn thông qua các điểm hiện diện (Edge Location) trên toàn cầu, giảm độ trễ.
    *   **Bảo mật:** Cung cấp một lớp bảo vệ bổ sung, che giấu địa chỉ thật của ALB và có thể tích hợp với AWS WAF (Web Application Firewall) sau này.

---

🔒 **Các bước thực hiện**

#### **1. Bắt đầu tạo Distribution**

*   Từ **AWS Management Console**, tìm kiếm và chọn dịch vụ **CloudFront**.
*   Nhấn vào **Create a CloudFront distribution**.

{{< figure src="/images/2.prerequisite/2.12-createcloudfrontbe/cf-dashboard.png" title="Bắt đầu tạo CloudFront Distribution" >}}

#### **2. Cấu hình Origin (Nguồn)**

*   **Origin domain:** Nhấn vào ô và chọn DNS name của Application Load Balancer `ors-alb` từ danh sách thả xuống.
*   **Protocol:** Chọn `HTTP only`.
*   **HTTP port:** Để mặc định là `80`.

{{% notice info %}}
**Luồng hoạt động:**
Người dùng `--(HTTPS)-->` **CloudFront** `--(HTTP, port 80)-->` **ALB**.
CloudFront sẽ xử lý kết nối HTTPS an toàn, sau đó "nói chuyện" với ALB bằng HTTP thông thường.
{{% /notice %}}

{{< figure src="/images/2.prerequisite/2.12-createcloudfrontbe/cf-origin-config.png" title="Cấu hình Origin là Application Load Balancer" >}}

#### **3. Cấu hình Default Cache Behavior**

*   Đây là phần cấu hình quan trọng nhất để đảm bảo API hoạt động đúng cách.
*   Trong tab **Behaviors**, chọn `Default (*)` và nhấn **Edit**.
*   **Cache policy:** Chọn `CachingDisabled`. Điều này rất quan trọng vì chúng ta không muốn CloudFront lưu cache các phản hồi của API (vốn có tính động và thay đổi liên tục).
*   **Origin request policy - optional:** Chọn `AllViewer`. Chính sách này sẽ chuyển tiếp tất cả thông tin từ người dùng (headers, query strings, cookies) đến ALB, đảm bảo backend nhận được đầy đủ dữ liệu cần thiết để xử lý yêu cầu.
*   **Web Application Firewall (WAF):** Chọn `Do not enable security protections` cho mục đích của workshop này.
*   Nhấn **Save changes**.

{{< figure src="/images/2.prerequisite/2.12-createcloudfrontbe/cf-behavior-config.png" title="Cấu hình Cache Behavior cho API" >}}

#### **4. Hoàn tất, triển khai và kiểm tra**

*   Cuộn xuống cuối trang và nhấn **Create distribution**.
*   Quá trình triển khai một distribution ra toàn bộ mạng lưới của CloudFront có thể mất từ 5 đến 15 phút. Bạn sẽ thấy trạng thái `Deploying`.

{{< figure src="/images/2.prerequisite/2.12-createcloudfrontbe/cf-deploying.png" title="Chờ CloudFront hoàn tất triển khai" >}}

*   Khi trạng thái chuyển sang hiển thị ngày **Last modified**, hãy sao chép **Distribution domain name** (ví dụ: `d12345abcdef.cloudfront.net`).

{{< figure src="/images/2.prerequisite/2.12-createcloudfrontbe/cf-copy-domain.png" title="Sao chép Domain Name của Distribution" >}}

*   Dán domain name này vào trình duyệt của bạn. Kết quả trả về phải giống hệt như khi bạn truy cập qua DNS của ALB: `Welcome to Dining Verse Backend API!`. Điều này xác nhận CloudFront đã được cấu hình thành công để làm proxy cho ALB của bạn.