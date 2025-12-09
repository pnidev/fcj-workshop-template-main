---
title: "Week 10 Worklog"
date: 2025-11-11
weight: 10
chapter: false
pre: " <b> 1.10. </b> "
---

### Week 10 Objectives:

* Nắm vững mô hình **kết nối Active Directory trong môi trường Hybrid**.
* Triển khai liên thông giữa **AWS Managed Microsoft AD và hệ thống AD nội bộ**.
* Tìm hiểu và sử dụng **AD Connector & Simple AD** trong các tình huống thực tế.
* Thực hành xử lý lỗi, giám sát và tối ưu hệ thống directory.

---

### Workload trong tuần:

| Day | Task                                                                                                                      | Start | Done | Reference |
|---|-----------------------------------------------------------------------------------------------------------------------------|-------|------|-----------|
| 1  | - Ôn tập kiến trúc **Hybrid Active Directory** <br> - Hiểu cơ chế Trust giữa Domain <br> - Thiết lập mô hình Trust cơ bản  | 11/11/2025 | 11/11/2025 | AWS Hybrid AD Docs |
| 2  | - Nghiên cứu **AD Connector** <br> - So sánh **Managed AD – AD Connector – Simple AD** <br> - Kết nối thử với AD nội bộ     | 12/11/2025 | 12/11/2025 | AWS Directory Service Overview |
| 3  | - Cấu hình **Cross-domain Auth** <br> - Tích hợp đăng nhập tập trung (SSO) <br> - Liên kết với AWS WorkSpaces               | 13/11/2025 | 13/11/2025 | AWS SSO Docs |
| 4  | - Thực hành **xử lý lỗi directory** <br> - Theo dõi trạng thái domain, replication <br> - Thiết lập cơ chế backup & rollback | 14/11/2025 | 14/11/2025 | AWS Managed AD Best Practices |
| 5  | - **Mini lab tổng kết:** <br> + Triển khai Hybrid AD hoàn chỉnh <br> + Thiết lập trust hai chiều <br> + Test auth & SSO <br> + Tổng hợp use case thực tế | 15/11/2025 | 15/11/2025 | AWS Enterprise Architecture Notes |

---

### Week 10 Outcomes:

* Nắm chắc nguyên tắc vận hành **Hybrid Directory Infrastructure**.
* Tự cấu hình **Domain Trust** giữa AWS và môi trường on-premises.
* Hiểu rõ khi nào dùng Managed AD, AD Connector hoặc Simple AD.
* Tích hợp xác thực tập trung (SSO) cho ứng dụng trong AWS.
* Có khả năng giám sát, debug và khôi phục directory khi phát sinh lỗi.

---

### Summary:

Đến cuối tuần 10, tôi đã có thể:

* Thiết kế mô hình Hybrid AD phục vụ bài toán doanh nghiệp.
* Kết nối Active Directory on-premises với AWS Directory Service.
* Triển khai SSO và xác thực liên domain cho ứng dụng.
* Lựa chọn dịch vụ Directory phù hợp với từng yêu cầu thực tế.
* Theo dõi, bảo trì và xử lý tình huống sự cố trong môi trường sản xuất.

---
