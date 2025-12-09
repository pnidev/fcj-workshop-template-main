---
title: "Week 5 Worklog"
date: 2025-10-07
weight: 5
chapter: false
pre: " <b> 1.5. </b> "
---

### Mục tiêu tuần 5:

* Hiểu cơ chế hoạt động của **Amazon RDS** và khi nào nên dùng CSDL quan hệ.
* Triển khai, cấu hình và kết nối thử nghiệm **RDS instance** trong môi trường VPC.
* Làm quen với **Amazon DynamoDB** – NoSQL với hiệu năng cao & tối ưu scale.
* So sánh ưu nhược điểm RDS vs DynamoDB để lựa chọn đúng ngữ cảnh hệ thống.

---

### Công việc thực hiện trong tuần:

| Ngày | Nhiệm vụ                                                                                                        | Bắt đầu     | Hoàn thành      | Tài liệu                           |
|---|-------------------------------------------------------------------------------------------------------------------|-------------|-----------------|------------------------------------|
| 1  | - Giới thiệu tổng quan **Amazon RDS** <br> - Tìm hiểu MySQL/PostgreSQL engines <br> - Multi-AZ, Read Replica hoạt động thế nào? | 07/10/2025 | 07/10/2025      | https://000005.awsstudygroup.com/vi/ |
| 2  | - Khởi tạo RDS instance đầu tiên <br> - Thiết lập Security Group truy cập hợp lệ <br> - Kết nối RDS từ EC2 qua MySQL Workbench/CLI | 08/10/2025 | 08/10/2025      | https://000005.awsstudygroup.com/vi/4-create-rds/ |
| 3  | - Tìm hiểu snapshot và cơ chế backup tự động <br> - Restore DB thử nghiệm từ snapshot <br> - Test khả năng phục hồi dữ liệu | 09/10/2025 | 09/10/2025      | https://000005.awsstudygroup.com/vi/6-backup/ |
| 4  | - Làm quen **DynamoDB** <br> - Kiến trúc NoSQL, Partition Key/Sort Key <br> - Tạo bảng và chạy thử truy vấn | 10/10/2025 | 10/10/2025      | https://000060.awsstudygroup.com/vi/ |
| 5  | - **Mini lab**: <br> + Tạo RDS MySQL và thao tác CRUD <br> + Tạo DynamoDB table, đọc/ghi dữ liệu <br> + So sánh tốc độ đọc ghi RDS vs DynamoDB <br> + Cleanup resource tránh phát sinh phí | 11/10/2025 | 11/10/2025 | https://000005.awsstudygroup.com/vi/5-deploy-app/ |

---

### Kết quả đạt được:

* Hiểu được cơ chế **RDS vận hành, replicate và backup dữ liệu**.
* Tạo RDS instance, mở kết nối và chạy truy vấn thử thành công.
* Biết cách khôi phục dữ liệu DB từ snapshot và tự động backup.
* Tạo bảng DynamoDB và thực hiện CRUD bằng console & SDK.
* Có cái nhìn thực tế hơn về hiệu năng giữa CSDL quan hệ và NoSQL.

---

### Tổng kết:

Sau tuần 5, tôi đã:

* Vận hành được RDS MySQL từ khâu tạo – kết nối – vận hành – backup.
* Biết sử dụng Security Group để giới hạn truy cập DB an toàn.
* Hiểu snapshot/restore dùng trong kịch bản sự cố hoặc rollback.
* Thiết kế bảng DynamoDB phù hợp với khối lượng truy vấn lớn.
* Đánh giá được khi nào nên chọn RDS (transaction mạnh) hay DynamoDB (scale lớn, latency thấp).

---
