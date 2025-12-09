---
title: "Week 3 Worklog"
date: 2025-09-23
weight: 3
chapter: false
pre: " <b> 1.3. </b> "
---

### Mục tiêu tuần 3:

* Hiểu rõ cơ chế hoạt động của **Amazon EC2** và các loại instance.
* Biết cách triển khai, cấu hình và truy cập **EC2 trong VPC** đã tạo.
* Tìm hiểu về **IAM Role gán cho EC2** để ứng dụng truy cập dịch vụ AWS an toàn.
* Trải nghiệm môi trường lập trình trực tiếp trên mây thông qua **AWS Cloud9**.

---

### Công việc thực hiện trong tuần:

| Ngày | Nhiệm vụ                                                                                                         | Bắt đầu    | Hoàn thành      | Tài liệu                         |
|---|--------------------------------------------------------------------------------------------------------------------|------------|-----------------|----------------------------------|
| 1  | - Tìm hiểu khái niệm **EC2** <br> - Phân loại instance theo nhu cầu CPU/RAM <br> - Làm quen với AMI và mục đích sử dụng | 23/09/2025 | 23/09/2025      | https://000003.awsstudygroup.com/vi/ |
| 2  | - Tạo instance EC2 đầu tiên <br> - Cấu hình Security Group ports (SSH/HTTP) <br> - SSH kết nối vào server           | 24/09/2025 | 24/09/2025      | https://000003.awsstudygroup.com/vi/ |
| 3  | - Tạo **IAM Role** truy cập S3 <br> - Gán Role cho EC2 thay vì dùng access key <br> - Kiểm tra quyền truy cập S3 trên máy chủ | 25/09/2025 | 25/09/2025 | https://000048.awsstudygroup.com/vi/ |
| 4  | - Tạo workspace với **AWS Cloud9** <br> - Làm quen UI, terminal, editor <br> - Chạy thử code nhỏ (Python/NodeJS)    | 26/09/2025 | 26/09/2025      | https://000049.awsstudygroup.com/vi/ |
| 5  | - **Mini Lab**: <br> + EC2 + IAM Role truy cập S3 <br> + Deploy web app mẫu trên Cloud9 <br> + Thực hành Stop/Terminate đúng quy trình | 27/09/2025 | 27/09/2025 | FCJ Internal Docs, AWS Blogs |

---

### Kết quả đạt được:

* Hiểu nguyên lý hoạt động của **EC2**, AMI và sự khác nhau giữa các loại instance.
* Tự tay tạo, SSH truy cập và điều khiển EC2 như một máy chủ thật.
* Cấu hình quyền truy cập **S3 bằng IAM Role** mà không cần dùng secret keys.
* Làm quen với **Cloud9** – viết code, deploy cơ bản ngay trên nền tảng AWS.
* Thành thạo thao tác quản lý vòng đời EC2: chạy – dừng – xoá tài nguyên tiết kiệm phí.

---

### Tổng kết:

Sau tuần 3, tôi đã có thể:

* Triển khai EC2 trong VPC và thiết lập bảo mật thông qua Security Group.
* Gán IAM Role giúp máy chủ truy cập dịch vụ AWS một cách an toàn.
* Tự kết nối vào server để cấu hình, chạy ứng dụng và quản lý tài nguyên.
* Sử dụng Cloud9 như IDE chính cho lập trình backend và deploy thử nghiệm.
* Nhận thức rõ hơn về chi phí EC2 và cách tối ưu để tránh bị tính phí ngoài ý muốn.

---
