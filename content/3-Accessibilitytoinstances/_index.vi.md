---
title : "Kết nối và Triển khai Ứng dụng"
date : "2025-09-06" 
weight : 3 
chapter : false
pre : " <b> 3. </b> "
---

Sau khi đã xây dựng xong toàn bộ hạ tầng trên AWS, bước tiếp theo là kết nối vào máy chủ EC2, cài đặt môi trường cần thiết, và triển khai mã nguồn ứng dụng backend.

Vì máy chủ `ors-ec2` ban đầu của chúng ta được đặt trong **Public Subnet** và đã được gán một IP công cộng, chúng ta có thể kết nối trực tiếp vào nó từ máy tính cá nhân bằng SSH để thực hiện các tác vụ cài đặt.

Quy trình trong chương này sẽ bao gồm:
1.  **Kết nối** vào EC2 instance bằng tệp `.pem` đã tạo.
2.  **Cài đặt** các công cụ cần thiết như Git, Node.js.
3.  **Tải mã nguồn** dự án từ GitHub về máy chủ.
4.  **Cấu hình** biến môi trường để kết nối tới cơ sở dữ liệu RDS.
5.  **Chạy ứng dụng** backend và kiểm tra hoạt động.

{{% notice info %}}
Trong chương này, **MobaXterm** sẽ là công cụ SSH client chính được sử dụng trên Windows để thực hiện các kết nối và chạy các dòng lệnh. Bạn cũng có thể sử dụng các công cụ khác như Terminal (macOS/Linux) hoặc PuTTY.
{{% /notice %}}