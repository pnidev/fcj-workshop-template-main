---
title: "Sư kiện 7"
date: "`r Sys.Date()`"
weight: 1
chapter: false
pre: " <b> 4.7. </b> "
---

# Báo Cáo Tóm Tắt: AWS Well-Architected Security Pillar (AWS Cloud Mastery Series #3)

## Mục Tiêu Sự Kiện
- Tìm hiểu sâu hơn về các dịch vụ IAM, IAM Identity Center, SSO, SCP  
- Học về CloudTrail, GuardDuty, Security Hub và tự động hóa bằng EventBridge  
- Bảo vệ hạ tầng: phân tách VPC, vị trí tài nguyên public vs private  
- Tìm hiểu về Bảo vệ Dữ liệu và Incident Response

### Diễn Giả
- **Le Vu Xuan Anh**: : Đội trưởng AWS Cloud Club HCMUTE – First Cloud AI Journey
- **Tran Duc Anh**: Đội trưởng AWS Cloud Club SGU – First Cloud AI Journey 
- **Tran Doan Cong Ly**: Đội trưởng AWS Cloud Club PTIT – First Cloud AI Journey 
- **Danh Hoang Hieu Nghi**: Đội trưởng AWS Cloud Club HUFLIT – First Cloud AI Journey 
- **Nguyen Tuan Thinh Cloud**: Thực tập sinh Kỹ sư Cloud – First Cloud AI Journey  
- **Nguyen Do Thanh Dat**: Thực tập sinh Kỹ sư Cloud – First Cloud AI Journey     
- **Mendel Grabski (Long)**: Cựu Head of Security & DevOps – Cloud Security Solution Architect  
- **Tinh Truong**: AWS Community Builder – Kỹ sư Nền tảng tại TymeX

## Những Điểm Nổi Bật

### Identity & Access Management
- Hiểu các khái niệm IAM cốt lõi: Users, Roles, Policies và lý do nên tránh long-term credentials.  
- Khám phá IAM Identity Center (SSO) và Permission Sets.  
- Nắm các thực hành bảo mật quan trọng: MFA, credential rotation, IAM Access Analyzer.  

### Detection
- Hiểu CloudTrail cung cấp auditing đầy đủ trên multi-account.  
- Tìm hiểu các dịch vụ phát hiện mối đe dọa: GuardDuty và Security Hub.  
- Nắm logging ở nhiều lớp: VPC Flow Logs, ALB Logs, S3 Access Logs.  
- Xây dựng cảnh báo và automation với EventBridge.  

### Infrastructure Protection
- KMS: key policies, grants, rotation  
- Mã hóa at-rest & in-transit: S3, EBS, RDS, DynamoDB  
- Secrets Manager & Parameter Store — các mẫu xoay vòng secrets  
- Phân loại dữ liệu và thiết lập access guardrails  

### Incident Response
- Vòng đời IR trên AWS  
- Xử lý khi IAM key bị lộ  
- Kiểm tra S3 public exposure  
- Phát hiện malware trên EC2  
- Snapshot, cô lập, thu thập bằng chứng  
- Tự động phản hồi bằng Lambda/Step Functions  

## Trải Nghiệm Sự Kiện
- Tham gia sự kiện **AWS Well-Architected Security Pillar**, mình không chỉ học thêm nhiều dịch vụ mới mà còn có cơ hội gặp gỡ các bạn từ nhiều trường đại học khác.  
- Học thêm về các dịch vụ bảo mật, biết thêm về cách xử lý sự cố thực tế.  
- Ngoài ra còn có cơ hội gặp chuyên gia nước ngoài đang làm việc tại AWS và lắng nghe chia sẻ kinh nghiệm của họ.  

#### Một Số Hình Ảnh Tại Sự Kiện
>Sự kiện giúp mình hiểu rõ hơn về bảo mật, có cơ hội tìm hiểu thêm về các sự cố và cách giải quyết. Đồng thời, mình cũng mong muốn tham gia CloudClub để có thêm nhiều trải nghiệm và kỷ niệm đẹp.
