---
title : "Tạo CloudFront cho Frontend (S3)"
date : "2025-09-06"
weight : 13
chapter : false
pre : " <b> 2.13 </b> "
---

### Tạo CloudFront Distribution cho Frontend (S3)

ℹ️ **Mục tiêu**

*   Tạo một CloudFront Distribution thứ hai, lần này dành riêng cho việc phân phối các tệp tĩnh của frontend từ S3 bucket.
*   **Tăng tốc độ tải trang:** CloudFront sẽ lưu trữ (cache) các tệp frontend (HTML, CSS, JS, images) tại các điểm biên (Edge Location) trên toàn cầu, giúp người dùng ở khắp nơi truy cập trang web với độ trễ thấp nhất.
*   **Tăng cường bảo mật:** Chúng ta sẽ cấu hình **Origin Access Identity (OAI)**. OAI là một "người dùng ảo" đặc biệt của CloudFront. Chúng ta sẽ cấp quyền cho OAI này đọc các tệp trong S3 bucket, sau đó khóa bucket lại, không cho phép bất kỳ truy cập công khai nào khác. Điều này đảm bảo mọi người dùng đều phải đi qua CloudFront để xem trang web của bạn.

---

🔒 **Các bước thực hiện**

#### **1. Bắt đầu tạo Distribution**

*   Trong **AWS Management Console**, quay trở lại dịch vụ **CloudFront**.
*   Nhấn vào **Create distribution**.

{{< figure src="/images/2.prerequisite/2.13-createcloudfrontfe/cf-dashboard.png" title="Bắt đầu tạo CloudFront Distribution mới" >}}

#### **2. Cấu hình Origin (Nguồn)**

*   **Origin domain:** Nhấn vào ô này và chọn S3 bucket của bạn từ danh sách: `ors-fe.s3.ap-southeast-1.amazonaws.com`.
*   **Origin access:** Đây là bước cấu hình bảo mật quan trọng nhất.
    *   Chọn `Legacy access identities`.
    *   **Origin access identity:** Nhấn vào **Create new OAI**.
    *   Để tên mặc định và nhấn **Create**.
    *   **Bucket policy:** Chọn `Yes, update the bucket policy`. Thao tác này sẽ tự động thêm một chính sách vào S3 bucket của bạn, cho phép OAI vừa tạo có quyền đọc các đối tượng.

{{< figure src="/images/2.prerequisite/2.13-createcloudfrontfe/cf-s3-oai-config.png" title="Cấu hình Origin là S3 Bucket và tạo OAI" >}}

#### **3. Cấu hình các mục còn lại**

*   **Web Application Firewall (WAF):** Chọn `Do not enable security protections`.
*   **Settings - Price Class:** Chọn `Use North America, Europe, Asia, Middle East, and Africa` để tối ưu chi phí và hiệu năng.
*   **Settings - Default root object:**
    *   Gõ `index.html`.
    {{% notice info %}}
    Đây là một thiết lập bắt buộc. Nó cho CloudFront biết phải trả về tệp nào khi người dùng truy cập vào tên miền gốc (ví dụ: `https://d...cloudfront.net/`) mà không chỉ định một tệp cụ thể.
    {{% /notice %}}

{{< figure src="/images/2.prerequisite/2.13-createcloudfrontfe/cf-s3-settings.png" title="Thiết lập các cài đặt bổ sung" >}}

#### **4. Hoàn tất, triển khai và kiểm tra**

*   Cuộn xuống dưới cùng và nhấn **Create distribution**.
*   Tương tự như trước, quá trình triển khai sẽ mất vài phút. Hãy chờ cho đến khi trạng thái không còn là `Deploying`.
*   Sao chép giá trị **Distribution domain name**.

{{< figure src="/images/2.prerequisite/2.13-createcloudfrontfe/cf-s3-copy-domain.png" title="Sao chép Domain Name của Distribution Frontend" >}}

*   Dán domain name này vào trình duyệt. Bạn sẽ thấy ứng dụng frontend của mình được tải lên thành công, nhanh chóng và an toàn thông qua mạng lưới của CloudFront!