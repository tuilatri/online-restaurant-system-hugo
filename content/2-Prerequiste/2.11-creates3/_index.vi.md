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

*   **Block Public Access settings for this bucket:**
    *   **Tích** ở ô **Block all public access**.

{{< figure src="/images/2.prerequisite/2.11-creates3/s3-unblock-public-access.png" title="Chặn truy cập công khai cho bucket" >}}

*   **Bucket Versioning:** Chọn `Enable`.
*   Cuộn xuống và nhấn **Create bucket**.

#### **3. Tải file Frontend lên Bucket**

*   Sau khi tạo bucket thành công, hãy vào bucket `ors-fe`.
*   Nhấn **Upload** và tải toàn bộ các tệp và thư mục của dự án frontend của bạn lên.

{{< figure src="/images/2.prerequisite/2.11-creates3/s3-upload-files.png" title="Tải các tệp Frontend lên S3" >}}

#### **4. Cấu hình Static Website Hosting**

*   Trong bucket `ors-fe`, chuyển sang tab **Properties**.
*   Cuộn xuống dưới cùng đến mục **Static website hosting** và nhấn **Edit**.
*   Chọn **Enable**.
*   **Index document:** `index.html`
*   Nhấn **Save changes**.

{{< figure src="/images/2.prerequisite/2.11-creates3/s3-static-hosting-01.png" title="Bật và cấu hình Static Website Hosting" >}}
