---
title: "Event 4"
date: "`r Sys.Date()`"
weight: 1
chapter: false
pre: " <b> 4.2. </b> "
---
# Summary Report: ​DevOps on AWS (AWS Cloud Mastery Series #2)

### Event Objectives
- Hiểu sâu hơn về tư duy DevOps và văn hoá triển khai
- Nắm rõ mô hình Infrastructure as Code và cách ứng dụng
- Mở rộng kiến thức về dịch vụ container trên AWS
- Cải thiện khả năng quan sát hệ thống, giám sát và logging

### Speakers
- **Truong Quang Trinh** – Kỹ sư Nền tảng – AWS Community Builder tại TymeX  
- **Bao Huynh** – AWS Community Builders  
- **Thinh Nguyen** – AWS Community Builders  
- **Vi Tran** – AWS Community Builders  
- **Long Huynh** – AWS Community Builders  
- **Quy Pham** – AWS Community Builders  
- **Nghiem Le** – AWS Community Builders  

---

## Key Highlights

### AWS CodeCommit
- Dịch vụ quản lý mã nguồn dựa trên Git chạy hoàn toàn trên AWS
- Hỗ trợ nhiều workflow: GitFlow, Trunk-Based và feature branch
- Phù hợp cho teamwork, code review và tích hợp CI/CD tự động

### AWS CodeBuild
- Build service do AWS quản lý—dùng để compile, test và đóng gói ứng dụng
- Cho phép tuỳ chỉnh buildspec để định nghĩa pipeline
- Tối ưu quy trình CI khi có thể chạy test tự động trong pipeline

### AWS CodeDeploy
- Tự động hoá quá trình triển khai lên EC2, Lambda hoặc môi trường On-premise
- Hỗ trợ nhiều chiến lược deploy: Blue/Green, Rolling, Canary
- Giảm thiểu downtime và hạn chế lỗi khi release

### AWS CodePipeline
- Công cụ điều phối CI/CD end-to-end (build → test → deploy)
- Tự động hoá release, hỗ trợ đa môi trường và rollback linh hoạt
- Thích hợp cho continuous delivery không cần thao tác thủ công

### AWS CloudFormation
- Quản lý hạ tầng bằng template (IaC) giúp tạo resource lặp lại, kiểm soát tốt hơn
- Có cơ chế rollback, drift detection để đảm bảo trạng thái đúng như cấu hình
- Tự động hoá cấp phát, giảm sai sót do thao tác tay

### AWS CDK (Cloud Development Kit)
- Viết hạ tầng bằng code (TypeScript/Python/JS) thay vì YAML/JSON
- Cung cấp construct template sẵn giúp triển khai nhanh hơn
- Biên dịch lại thành CloudFormation nên dùng trong production an toàn

### Container Services on AWS
- Giải thích cơ bản Docker và container hoá microservices
- Amazon ECR dùng lưu trữ image + quét bảo mật + retention policy
- ECS/EKS để chạy container scale lớn, tự động điều phối workload
- App Runner cho phép deploy ứng dụng container đơn giản không cần quản lý hạ tầng

### Monitoring & Observability
- CloudWatch: quản lý log, metric, alarm và dashboard
- AWS X-Ray dùng để trace request, phân tích hiệu năng từng service
- Tăng khả năng theo dõi hệ thống từ backend → frontend

### DevOps Best Practices & Case Studies
- Triển khai nhiều chiến lược release: feature flag, A/B test, canary deployment
- Khuyến khích tự động hoá test trong CI/CD
- Tìm hiểu quy trình xử lý sự cố và viết postmortem chuẩn
- Nêu case study thực tế từ startup đến doanh nghiệp lớn

---

## Event Experience
- Qua sự kiện **DevOps on AWS**, mình có cơ hội củng cố lại kiến thức về DevOps cũng như hiểu rõ hơn cách vận hành các
