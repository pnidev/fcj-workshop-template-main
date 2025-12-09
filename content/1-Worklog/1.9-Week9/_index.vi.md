---
title: "Week 9 Worklog"
date: 2025-11-04
weight: 9
chapter: false
pre: " <b> 1.9. </b> "
---

### Mục tiêu tuần 9:

* Tiếp cận mô hình vận hành **Windows Workloads trên AWS**.
* Khởi tạo và quản trị **Windows Server chạy trên EC2**.
* Làm quen với **AWS Managed Microsoft AD** để quản lý danh tính tập trung.
* Thử nghiệm tích hợp Windows với các dịch vụ AWS phục vụ doanh nghiệp.

---

### Công việc thực hiện trong tuần:

| Ngày | Nhiệm vụ                                                                                                                             | Bắt đầu     | Hoàn thành      | Tài liệu tham khảo                                        |
|---|----------------------------------------------------------------------------------------------------------------------------------------|-------------|-----------------|-----------------------------------------------------------|
| 1  | - Ôn tập kiến thức **Windows on AWS** <br> - Hiểu cơ chế license Windows khi chạy EC2 <br> - Tạo EC2 Windows Server đầu tiên          | 04/11/2025  | 04/11/2025      | https://000093.awsstudygroup.com/vi/                      |
| 2  | - Kết nối Windows EC2 bằng **Remote Desktop (RDP)** <br> - Cài đặt roles/features cần thiết <br> - Triển khai IIS Web Server trên Windows | 05/11/2025  | 05/11/2025      | FCJ Notes, AWS Windows Hands-on                           |
| 3  | - Nghiên cứu **AWS Managed Microsoft AD** <br> - Nắm cấu trúc AD, Domain, OU, Group <br> - Tạo directory service trên AWS             | 06/11/2025  | 06/11/2025      | AWS Directory Service Docs                                |
| 4  | - **Join Windows EC2 vào Domain** <br> - Tạo user, group & phân quyền <br> - Cấu hình Group Policy để quản lý tập trung               | 07/11/2025  | 07/11/2025      | AWS Managed AD Whitepaper                                 |
| 5  | - **Bài thực hành tổng hợp:** <br> + Deploy ứng dụng Windows lên EC2 <br> + Tích hợp Managed AD cho xác thực tập trung <br> + Test đăng nhập domain + SSO <br> + Xây dựng quy trình quản trị user | 08/11/2025 | 08/11/2025      | AWS Enterprise Patterns, FCJ Workshop                     |

---

### Kết quả đạt được:

* Khởi tạo và quản lý thành công **Windows Server trên EC2**.
* Kết nối bằng RDP, cài đặt IIS và triển khai ứng dụng mẫu trên Windows.
* Tạo và vận hành **AWS Managed Microsoft AD** phục vụ quản lý danh tính.
* Gắn Windows EC2 vào domain, quản lý User/Group tập trung.
* Thực hành quản trị cơ bản qua Group Policy và SSO.

---

### Tổng kết:

Kết thúc tuần 9, tôi đã có thể:

* Tự triển khai workload Windows Server trên AWS.
* Cài đặt IIS và host ứng dụng trực tiếp trên Windows EC2.
* Xây dựng Directory Service bằng Managed AD và áp dụng vào thực tế.
* Join máy chủ vào domain để quản lý user và authentication.
* Vận hành môi trường Windows trong AWS với hướng tiếp cận chuẩn doanh nghiệp.

---
