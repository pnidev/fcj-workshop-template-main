---
title: "Week 11 Worklog"
date: 2025-11-18
weight: 11
chapter: false
pre: " <b> 1.11. </b> "
---

### Week 11 Objectives:

* Hiểu sâu hơn về mô hình **High Availability (HA)** trong hệ thống cloud.
* Thiết kế và vận hành **kiến trúc web nhiều tầng (multi-tier)**.
* Làm quen và so sánh các loại **Elastic Load Balancer** (ALB, NLB, CLB).
* Triển khai cơ chế **backup, replication & disaster recovery** cho ứng dụng.

---

### Weekly Task Breakdown:

| Day | Task                                                                                                                           | Start | Done | References |
|---|----------------------------------------------------------------------------------------------------------------------------------|-------|------|------------|
| 1  | - Tổng quan HA trong cloud <br> - Khái niệm fault tolerance, resilience <br> - Nghiên cứu Reliability Pillar trong Well-Architected | 18/11/2025 | 18/11/2025 | AWS Well-Architected Docs |
| 2  | - Học ELB chi tiết <br> - Phân biệt ALB, NLB, CLB và use case từng loại <br> - Tạo thử Application Load Balancer                 | 19/11/2025 | 19/11/2025 | AWS ELB Service Guide |
| 3  | - Thiết kế kiến trúc 3-tier Web → App → DB <br> - Cấu hình load balancing Multi-AZ <br> - Xây môi trường demo nhiều lớp          | 20/11/2025 | 20/11/2025 | AWS Multi-Tier Architecture Notes |
| 4  | - Triển khai **backup & restore** cho RDS, EBS <br> - Thử nghiệm cross-region replication <br> - Lập kế hoạch DR Strategy        | 21/11/2025 | 21/11/2025 | AWS DR Whitepaper |
| 5  | - **Lab tổng hợp:** <br> + Build HA Web App trên AWS <br> + ALB + Auto Scaling hoạt động ổn định <br> + Multi-AZ RDS + Read Replica <br> + Test failover DR scenario | 22/11/2025 | 22/11/2025 | FCJ Hands-on Workshop |

---

### Week 11 Outcomes:

* Nắm rõ kiến trúc **High Availability & Fault Tolerant** trong thực tế.
* Tự cấu hình và thử nghiệm các loại **Elastic Load Balancer**.
* Hoàn thiện mô hình 3 lớp với khả năng mở rộng và chống lỗi.
* Xây dựng kế hoạch DR & sao lưu dữ liệu an toàn.
* Tạo được ứng dụng hoạt động bền vững khi xảy ra sự cố hệ thống.

---

### Summary:

Kết thúc tuần này, tôi đã:

* Biết xây dựng hạ tầng web với HA đúng chuẩn AWS.
* Vận hành Application Load Balancer để phân phối traffic linh hoạt.
* Tạo kiến trúc web multi-tier có khả năng scale và multi-AZ.
* Cài đặt backup automation và cơ chế khôi phục sự cố (DR).
* Kiểm tra failover thực tế giúp hệ thống sẵn sàng chạy production.

---
