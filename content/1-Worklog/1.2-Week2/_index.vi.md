---
title: "Week 2 Worklog"
date: 2025-09-16
weight: 2
chapter: false
pre: " <b> 1.2. </b> "
---

### Mục tiêu tuần 2:

* Tìm hiểu nền tảng **AWS Networking** thông qua Amazon VPC.
* Nắm rõ các thành phần trong VPC: **Subnets, Route Tables, Internet Gateway, Security Group, NACL**.
* Thực hành workshop thiết kế mạng nội bộ, tạo kết nối internet trong AWS.
* Làm quen với **AWS CLI** để thao tác tài nguyên bằng dòng lệnh thay vì giao diện web.

---

### Công việc thực hiện trong tuần:

| Ngày | Công việc                                                                                                        | Bắt đầu     | Hoàn thành     | Tài liệu                           |
|---|--------------------------------------------------------------------------------------------------------------------|------------|----------------|------------------------------------|
| 1  | - Tổng quan về **Amazon VPC** <br> - Xác định dải CIDR phù hợp <br> - Tạo VPC mặc định và truy cập thử nghiệm      | 16/09/2025 | 16/09/2025     | https://000003.awsstudygroup.com/vi/ |
| 2  | - Tạo **Public Subnet & Private Subnet** <br> - Liên kết Route Table <br> - Gán Internet Gateway cho public subnet | 17/09/2025 | 17/09/2025     | https://000004.awsstudygroup.com/vi/ |
| 3  | - Thiết lập Security Group cơ bản <br> - Tìm hiểu cơ chế Network ACL <br> - So sánh SG vs NACL ứng dụng thực tế     | 18/09/2025 | 18/09/2025     | https://000003.awsstudygroup.com/vi/ |
| 4  | - Thực hành workshop Networking <br> - Cấu hình NAT Gateway cho private subnet <br> - Thử nghiệm VPC Peering       | 19/09/2025 | 19/09/2025     | https://000003.awsstudygroup.com/vi/ |
| 5  | - Cài đặt **AWS CLI** <br> - Test CLI commands cơ bản <br> **Mini Lab:** <br> + Tạo VPC qua CLI <br> + Liệt kê tài nguyên <br> + Xoá VPC bằng CLI | 20/09/2025 | 20/09/2025 | https://000011.awsstudygroup.com/vi/ |

---

### Kết quả đạt được:

* Hiểu cách hoạt động của **VPC và các thành phần mạng trong AWS**.
* Thiết lập được Subnet, Route Table và Internet Gateway một cách thủ công.
* Nhận biết rõ sự khác biệt giữa **Security Group (stateful) và Network ACL (stateless)**.
* Hoàn thành lab networking – thao tác cấu hình kết nối nội bộ và internet.
* Cài đặt AWS CLI thành công và sử dụng để tạo/xoá tài nguyên thay vì thao tác thủ công.

---

### Tổng kết:

Kết thúc tuần 2, tôi đã:

* Tự xây dựng một VPC theo cấu trúc chuẩn gồm public/private subnet.
* Tạo routing để public subnet ra internet và private subnet đi qua NAT Gateway.
* Áp dụng security bằng cả Security Group lẫn NACL.
* Biết chạy lệnh CLI để quản lý tài nguyên nhanh, chính xác hơn so với console.
* Có nền tảng tốt cho những tuần sau liên quan đến EC2, LoadBalancer và Auto Scaling.

---
