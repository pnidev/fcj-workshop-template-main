---
title: "Week 8 Worklog"
date: 2025-10-28
weight: 8
chapter: false
pre: " <b> 1.8. </b> "
---

### Mục tiêu tuần 8:

* Tìm hiểu hệ thống **Amazon Route 53** và cách quản lý DNS trong môi trường cloud.
* Làm quen với **Amazon CloudFront** – dịch vụ CDN giúp phân phối nội dung nhanh hơn.
* Nắm được cơ chế hoạt động của **Lambda@Edge** để xử lý logic ngay tại Edge Location.
* Xây dựng mô hình phân phối nội dung toàn cầu với độ trễ thấp và độ tin cậy cao.

---

### Công việc thực hiện trong tuần:

| Ngày | Nhiệm vụ                                                                                                                | Bắt đầu     | Hoàn thành      | Tài liệu tham khảo              |
|---|----------------------------------------------------------------------------------------------------------------------------|-------------|-----------------|---------------------------------|
| 1  | - Tổng quan **Route 53** <br> - Ôn lại khái niệm DNS, bản ghi A, CNAME, alias <br> - Tạo hosted zone và record đơn giản   | 28/10/2025  | 28/10/2025      | https://000010.awsstudygroup.com/vi/ |
| 2  | - Áp dụng các routing policy: Simple, Weighted, Failover <br> - Cài đặt Health Check để thực hiện dự phòng DNS           | 29/10/2025  | 29/10/2025      | https://000010.awsstudygroup.com/vi/2-prerequiste/ |
| 3  | - Làm quen **CloudFront CDN** <br> - Tìm hiểu Edge Location & Cache TTL <br> - Tạo CloudFront Distribution đầu tiên       | 30/10/2025  | 30/10/2025      | https://000094.awsstudygroup.com/vi/ |
| 4  | - Kết nối CloudFront với **S3 origin** <br> - Gán tên miền riêng thông qua Route 53 <br> - Cấp chứng chỉ SSL bằng ACM     | 31/10/2025  | 31/10/2025      | https://000094.awsstudygroup.com/vi/1.-cloud-front-với-s3/ |
| 5  | - Tìm hiểu **Lambda@Edge** <br> - Thực hành: Tạo CloudFront + domain riêng + SSL <br> - Triển khai Lambda@Edge xử lý request/response <br> - Đánh giá tốc độ và khả năng phân phối của CDN | 01/11/2025 | 01/11/2025      | https://000130.awsstudygroup.com/vi/ |

---

### Kết quả đạt được:

* Hiểu cách quản lý DNS, bản ghi, Hosted Zone bằng Route 53.
* Thiết lập Routing Policies phục vụ high-availability và failover.
* Triển khai CloudFront thành công để phân phối nội dung toàn cầu.
* Ứng dụng Lambda@Edge để xử lý logic ngay tại biên mạng.
* Kết hợp Route 53 + CloudFront + SSL + Edge Computing thành một CDN hoàn chỉnh.

---

### Tổng kết:

Kết thúc tuần 8, tôi đã có thể:

* Tối ưu phân phối nội dung với Route 53 và CloudFront.
* Tạo chứng chỉ SSL/TLS bằng ACM và cấu hình cho domain tùy chỉnh.
* Sử dụng Lambda@Edge để tùy biến phản hồi người dùng theo request headers/path.
* Xây dựng mô hình CDN giảm độ trễ truy cập cho người dùng ở nhiều khu vực khác nhau.
* Có cái nhìn tổng quan về kiến trúc global scale phục vụ web/app real-time.

---
