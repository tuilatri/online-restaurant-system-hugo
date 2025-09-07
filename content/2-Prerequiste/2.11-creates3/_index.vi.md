---
title : "Tạo S3 Bucket cho Frontend"
date : "2025-09-06"
weight : 11
chapter : false
pre : " <b> 2.11 </b> "
---

### Tạo S3 Bucket để lưu trữ Frontend

ℹ️ **Mục tiêu**

*   **Amazon S3 (Simple Storage Service)** là một dịch vụ lưu trữ đối tượng có khả năng mở rộng, độ bền cao và chi phí thấp.
*   Chúng ta sẽ sử dụng S3 để lưu trữ toàn bộ các tệp tĩnh của ứng dụng frontend (HTML, CSS, JavaScript, hình ảnh).
*   Sau đó, chúng ta sẽ cấu hình bucket này để hoạt động như một máy chủ web tĩnh (**Static Website Hosting**).

---

🔒 **Các bước thực hiện**

#### **1. Bắt đầu tạo S3 Bucket**

*   Trong **AWS Management Console**, tìm kiếm và chọn dịch vụ **S3**.
*   Nhấn vào **Create bucket**.

{{< figure src="/images/2.prerequisite/2.11-creates3/s3-dashboard.png" title="Bắt đầu tạo S3 Bucket" >}}

#### **2. Cấu hình Bucket**

*   **Bucket name:** `ors-fe`
    {{% notice warning %}}
    Tên S3 bucket là duy nhất trên toàn cầu. Nếu tên `ors-fe` đã được người khác sử dụng, bạn cần thêm các ký tự hoặc số để làm cho nó trở nên độc nhất (ví dụ: `ors-fe-haminhtri-0309`).
    {{% /notice %}}

*   **AWS Region:** Chọn Region bạn đang làm việc.

*   **Object Ownership:** Chọn `ACLs enabled`.
    {{% notice info %}}
    Chúng ta cần bật Access Control Lists (ACL) để chuẩn bị cho các bước cấu hình truy cập công khai tạm thời hoặc khi tích hợp với một số dịch vụ khác. Đây là một bước cần thiết theo ghi chú của bạn.
    {{% /notice %}}

{{< figure src="/images/2.prerequisite/2.11-creates3/s3-config-acl.png" title="Cấu hình tên, Region và bật ACLs" >}}

*   **Block Public Access settings for this bucket:**
    *   **Bỏ tích** ở ô **Block all public access**.
    *   Tích vào ô xác nhận: `I acknowledge that the current settings might result in this bucket and the objects within becoming public.`

{{% notice warning %}}
Chúng ta tạm thời cho phép truy cập công khai để có thể kiểm tra trang web tĩnh sau khi cấu hình. Ở bước tạo CloudFront tiếp theo, chúng ta sẽ thiết lập **Origin Access Identity (OAI)** để bảo vệ bucket này một cách an toàn hơn, lúc đó bạn có thể bật lại **Block all public access**.
{{% /notice %}}

{{< figure src="/images/2.prerequisite/2.11-creates3/s3-unblock-public-access.png" title="Bỏ chặn truy cập công khai cho bucket" >}}

*   **Bucket Versioning:** Chọn `Enable`.
*   Cuộn xuống và nhấn **Create bucket**.

#### **3. Tải file Frontend lên Bucket**

*   Sau khi tạo bucket thành công, hãy vào bucket `ors-fe`.
*   Nhấn **Upload** và tải toàn bộ các tệp và thư mục của dự án frontend của bạn lên.

{{< figure src="/images/2.prerequisite/2.11-creates3/s3-upload-files.png" title="Tải các tệp Frontend lên S3" >}}

#### **4. Cấp quyền truy cập công khai cho các đối tượng**

*   Sau khi tải lên hoàn tất, trong bucket, chọn tất cả các tệp và thư mục.
*   Nhấn menu **Actions** và chọn **Make public using ACL**.
*   Xác nhận bằng cách nhấn **Make public**.

{{< figure src="/images/2.prerequisite/2.11-creates3/s3-make-public.png" title="Cấp quyền đọc công khai cho các tệp đã tải lên" >}}

#### **5. Cấu hình Static Website Hosting**

*   Trong bucket `ors-fe`, chuyển sang tab **Properties**.
*   Cuộn xuống dưới cùng đến mục **Static website hosting** và nhấn **Edit**.
*   Chọn **Enable**.
*   **Index document:** `index.html`
*   Nhấn **Save changes**.

{{< figure src="/images/2.prerequisite/2.11-creates3/s3-static-hosting.png" title="Bật và cấu hình Static Website Hosting" >}}

*   Sau khi lưu, quay lại mục **Static website hosting**, bạn sẽ thấy một URL điểm cuối (endpoint). Bạn có thể truy cập URL này để kiểm tra xem trang web frontend đã hoạt động hay chưa.

{{< figure src="/images/2.prerequisite/2.11-creates3/s3-get-url.png" title="Lấy URL của trang web tĩnh để kiểm tra" >}}