---
title : "Kết nối, Cài đặt và Triển khai"
date : "2025-09-06" 
weight : 1 
chapter : false
pre : " <b> 3.1. </b> "
---

### Hướng dẫn chi tiết các lệnh trên MobaXterm

Trong phần này, bạn sẽ thực hiện các lệnh để kết nối vào máy chủ `ors-ec2`, sau đó cài đặt môi trường và triển khai ứng dụng backend Node.js.

{{% notice info %}}
**Lưu ý:** Các lệnh được thực hiện tuần tự. Hãy đảm bảo bạn đã hoàn thành bước trước rồi mới tiếp tục bước sau.
{{% /notice %}}

#### **1. Kết nối vào EC2 Instance**
Đây là bước đầu tiên, kết nối từ máy tính của bạn vào máy chủ `ors-ec2` trong Public Subnet.

*   Mở MobaXterm và tạo một phiên **SSH** mới.
*   **Remote host:** Nhập địa chỉ **Public IPv4** của `ors-ec2` (bạn có thể lấy từ EC2 Dashboard).
*   **Specify username:** `ec2-user`.
*   **Use private key:** Chọn file `ors-keypair.pem` bạn đã tải về.
*   **Phân quyền cho file key (nếu dùng Terminal trên macOS/Linux):**
    Trước khi kết nối, bạn cần chạy lệnh này trên máy tính cá nhân để bảo mật file key:
    ```bash
    chmod 400 ors-keypair.pem
    ```

{{< figure src="/images/3.connect/3.1-deploy/mobaxterm-connect.png" title="Thiết lập phiên SSH trong MobaXterm" >}}

#### **2. Cài đặt Môi trường trên Server**
Khi đã kết nối thành công vào máy chủ, hãy cài đặt các công cụ cần thiết.

*   **Cập nhật hệ thống:**
    ```bash
    sudo yum update -y
    ```
    {{< figure src="/images/3.connect/3.1-deploy/yum-update.png" title="Cập nhật các gói hệ thống" >}}

*   **Cài đặt Git:**
    ```bash
    sudo yum install -y git
    ```

*   **Sao chép mã nguồn dự án từ GitHub:**
    ```bash
    git clone https://github.com/tuilatri/online-restaurant-system.git
    ```
    {{< figure src="/images/3.connect/3.1-deploy/git-clone.png" title="Sao chép dự án" >}}

*   **Di chuyển vào thư mục backend:**
    ```bash
    cd online-restaurant-system/backend
    ```

*   **Cài đặt Node.js và npm:**
    ```bash
    sudo dnf install -y nodejs
    ```
    {{< figure src="/images/3.connect/3.1-deploy/install-nodejs.png" title="Cài đặt Node.js" >}}

*   **Kiểm tra phiên bản để xác nhận cài đặt thành công:**
    ```bash
    node -v
    npm -v
    ```

#### **3. Cài đặt và Cấu hình Ứng dụng**

*   **Cài đặt các thư viện phụ thuộc của dự án:**
    ```bash
    npm install
    ```
    {{< figure src="/images/3.connect/3.1-deploy/npm-install.png" title="Cài đặt các gói npm" >}}

*   **Di chuyển vào thư mục `src` và tạo file cấu hình biến môi trường:**
    ```bash
    cd src
    nano .env
    ```

*   **Nhập thông tin kết nối cơ sở dữ liệu:**
    Trong trình soạn thảo `nano`, dán nội dung sau và thay thế bằng thông tin của bạn.
    ```env
    DB_HOST='your-rds-endpoint'
    DB_USER='admin'
    DB_PASSWORD='your-db-password'
    DB_DATABASE='ors'
    DB_PORT=3306
    ```
    {{< figure src="/images/3.connect/3.1-deploy/nano-env.png" title="Cấu hình file .env" >}}

*   **Lưu và thoát khỏi `nano`:**
    1.  Nhấn `Ctrl + O`
    2.  Nhấn `Enter` để xác nhận tên file.
    3.  Nhấn `Ctrl + X` để thoát.

#### **4. Khởi chạy và Kiểm tra Backend**

*   **Quay lại thư mục backend gốc:**
    ```bash
    cd ..
    ```

*   **Khởi chạy ứng dụng:**
    ```bash
    npm run start
    ```
    {{< figure src="/images/3.connect/3.1-deploy/npm-start.png" title="Ứng dụng backend đang chạy trên cổng 5000" >}}

*   **Kiểm tra hoạt động của ứng dụng ngay trên máy chủ:**
    Mở một cửa sổ terminal **mới** trên MobaXterm và kết nối lại vào cùng EC2 instance, sau đó dùng lệnh `curl` để kiểm tra.
    ```bash
    curl http://127.0.0.1:5000/api
    ```
    Nếu bạn nhận được phản hồi `Welcome to Dining Verse Backend API!`, nghĩa là ứng dụng đã chạy thành công!

#### **5. Hướng dẫn xử lý sự cố (Troubleshooting)**
Nếu bạn cập nhật code trên GitHub nhưng thay đổi không xuất hiện trên web, hãy làm theo các bước sau trên máy chủ:

*   **Di chuyển vào thư mục backend:**
    ```bash
    cd online-restaurant-system/backend
    ```

*   **Kéo code mới nhất về:**
    ```bash
    git pull origin main
    ```
*   **Tìm tiến trình (process) Node.js đang chạy:**
    ```bash
    ps aux | grep node
    ```
*   **Dừng tiến trình cũ bằng PID của nó:**
    ```bash
    kill -9 <PID>
    ```
    *Ví dụ thực tế:*
    ```
    [ec2-user@ip-10-0-144-101 backend]$ ps aux | grep node
    ec2-user    3117  0.0  6.6 1127836 64896 pts/1   Sl+   10:15   0:00 node server.js
    
    [ec2-user@ip-10-0-144-101 backend]$ kill -9 3117
    ```

*   **Khởi động lại ứng dụng:**
    ```bash
    npm run start
    ```