---
title: "Week 6 Worklog"
date: 2025-10-14
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
---

### Mục tiêu tuần 6:

* Làm quen với **Amazon ElastiCache** và vai trò của cache trong hệ thống phân tán.
* Hiểu cách ứng dụng Redis/Memcached để tăng tốc độ truy vấn dữ liệu.
* Tích hợp thử nghiệm ElastiCache với các dịch vụ đã học tuần trước.
* Ôn tập lại kiến thức Database (RDS + DynamoDB) để kết hợp với Cache phục vụ bài toán hiệu năng.

---

### Nhiệm vụ thực hiện trong tuần:

| Ngày | Công việc                                                                                                                | Bắt đầu     | Hoàn thành      | Tài liệu tham khảo               |
|---|----------------------------------------------------------------------------------------------------------------------------|-------------|-----------------|----------------------------------|
| 1  | - Tổng quan **ElastiCache** <br> - Sự khác nhau giữa Redis & Memcached <br> - Tư duy cache-first và cache-aside pattern   | 14/10/2025 | 14/10/2025      | https://000061.awsstudygroup.com/vi/1-introduce/ |
| 2  | - Khởi tạo **Redis Cluster** trên ElastiCache <br> - Cấu hình Security Group kết nối VPC <br> - Kết nối thử từ EC2         | 15/10/2025 | 15/10/2025      | https://000061.awsstudygroup.com/vi/3-amazonelasticacheforredis/3.4-grantaccesstocluster/ |
| 3  | - Thêm lớp Cache vào ứng dụng <br> - Cache Invalidation (TTL, manual invalidation) <br> - Theo dõi chỉ số hit/miss         | 16/10/2025 | 16/10/2025      | AWS Caching Guidelines, FCJ Notes |
| 4  | - **Bài lab tích hợp thực tế:** <br>  + Kết nối RDS + Redis <br>  + Lưu session & cache query <br>  + Test tốc độ phản hồi | 17/10/2025 | 17/10/2025      | AWS Architecture Labs |
| 5  | - **Mini practice:** <br>  + Deploy hệ thống 3-tier (EC2 → Redis → RDS) <br>  + Benchmark tốc độ có/không dùng cache <br>  + Review lại kiến trúc DB & tối ưu chi phí <br>  + Thu dọn tài nguyên tránh phát sinh phí | 18/10/2025 | 18/10/2025 | Well-Architected Framework, FCJ Session |

---

### Kết quả đạt được:

* Nắm được cách hoạt động của **In-memory Cache** và lý do hệ thống lớn luôn cần Redis.
* Phân biệt rõ ràng giữa **Redis** (session/state cache) và **Memcached** (memory-key store đơn giản).
* Tự xây dựng **Caching Layer** để giảm số lượng truy vấn DB.
* Kết hợp ElastiCache + RDS → tốc độ truy vấn cải thiện đáng kể.
* Hoàn thiện demo hệ thống web có khả năng scale và tối ưu tải.

---

### Tổng kết:

Kết thúc tuần 6, tôi đã có thể:

* Tự tạo và kết nối ElastiCache Cluster trong VPC riêng.
* Thiết kế cơ chế cache hợp lý cho API và database truy vấn thường xuyên.
* Tích hợp Redis vào ứng dụng để lưu session/login state, query caching.
* Đo đạc hiệu năng và đánh giá mức giảm tải khi sử dụng cache.
* Có cái nhìn rõ ràng hơn về việc kết hợp **RDS + DynamoDB + Redis** trong kiến trúc thực tế.

---
