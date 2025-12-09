---
title: "Week 7 Worklog"
date: 2025-10-21
weight: 7
chapter: false
pre: " <b> 1.7. </b> "
---

### Mục tiêu tuần 7:

* Tìm hiểu cơ chế **EC2 Auto Scaling** nhằm duy trì hiệu năng ứng dụng khi tải tăng hoặc giảm.
* Khởi tạo và cấu hình **Auto Scaling Group** kết hợp Launch Template.
* Làm quen **Amazon CloudWatch** để theo dõi tài nguyên theo thời gian thực.
* Thiết lập **scaling policies tự động** dựa trên chỉ số giám sát.

---

### Công việc thực hiện trong tuần:

| Ngày | Nhiệm vụ                                                                                                       | Bắt đầu     | Hoàn thành      | Tài liệu tham khảo              |
|---|------------------------------------------------------------------------------------------------------------------|-------------|-----------------|---------------------------------|
| 1  | - Nắm khái niệm **Auto Scaling** <br> - Phân loại chính sách scaling: Target tracking / Step / Scheduled <br> - Tìm hiểu Launch Template | 21/10/2025 | 21/10/2025 | https://000006.awsstudygroup.com/vi/ |
| 2  | - Tạo **Launch Template** tùy chỉnh <br> - Khởi tạo **Auto Scaling Group** trong VPC <br> - Cấu hình Min–Desired–Max Capacity | 22/10/2025 | 22/10/2025 | https://000006.awsstudygroup.com/vi/6-create-auto-scaling-group/ |
| 3  | - Nghiên cứu **CloudWatch Metrics & Namespaces** <br> - Tạo Dashboard theo dõi hoạt động EC2                         | 23/10/2025 | 23/10/2025 | https://000008.awsstudygroup.com/vi/ |
| 4  | - Tạo **CloudWatch Alarm** theo CPU Utilization <br> - Kết nối cảnh báo qua SNS email <br> - Gán alarm cho scaling policy | 24/10/2025 | 24/10/2025 | https://000008.awsstudygroup.com/vi/5-cloud-watch-alarm/ |
| 5  | - **Bài thực hành tổng hợp:** <br> + Tạo Auto Scaling Group + Policy tự động <br> + Tải test để kích hoạt scaling <br> + Theo dõi sự thay đổi instance trên CloudWatch Activity Log <br> + Thu hồi tài nguyên sau khi kiểm thử | 25/10/2025 | 25/10/2025 | FCJ Training Notes, Monitoring Best Practices |

---

### Kết quả đạt được:

* Hiểu sâu hơn về cách hoạt động của **EC2 Auto Scaling** và các loại policy khác nhau.
* Tự tạo và cấu hình **Launch Template + Auto Scaling Group** phục vụ load thực tế.
* Xây dựng Dashboard quan sát **CPU, Network, Status Checks** trên CloudWatch.
* Thiết lập cảnh báo tự động thông qua **CloudWatch Alarm + SNS Notification**.
* Mô phỏng tải và quan sát hệ thống tự scale-out/scale-in hiệu quả.

---

### Tổng kết:

Sau tuần 7, tôi đã có thể:

* Tự triển khai hệ thống Auto Scaling để duy trì hiệu năng dịch vụ.
* Xác định và chọn **scaling policy phù hợp** theo nhu cầu tải ứng dụng.
* Theo dõi tài nguyên bằng Dashboard và sử dụng cảnh báo để phản hồi kịp thời.
* Thử nghiệm tình huống tăng tải và quan sát cơ chế scale tự động.
* Tối ưu hướng vận hành EC2 theo tiêu chí tiết kiệm chi phí và đảm bảo uptime.

---
