---
title: "Proposal"
date: "`r Sys.Date()`"
weight: 2
chapter: false
pre: " <b> 2. </b> "
---
# Serverless Multiplayer Game Backend  
Một giải pháp AWS có khả năng mở rộng cho game thời gian thực & xử lý AI Avatar

---

## 1. Tóm tắt điều hành
Mục tiêu dự án là xây dựng **backend serverless cho game multiplayer Unity**, hướng đến khả năng mở rộng, độ ổn định cao và tự động hóa triển khai.  
Hệ thống được phân chia theo vai trò rõ ràng:

- **DevOps** – thiết lập hạ tầng AWS & CI/CD  
- **Backend (BE)** – xử lý API, dữ liệu gameplay và avatar  
- **Frontend (Unity)** – tương tác Cognito + WebSocket + REST API

Các dịch vụ AWS chính được sử dụng:

- **Amazon Cognito** – đăng nhập & quản lý người dùng  
- **AWS Lambda** – xử lý logic gameplay  
- **API Gateway WebSocket + DynamoDB Streams** – leaderboard thời gian thực  
- **Lambda Container (OpenCV/MediaPipe)** – xử lý avatar bằng AI  

Kiến trúc này giúp giảm chi phí, tăng tốc phát triển và phù hợp cho triển khai WebGL trên itch.io/CloudFront.

---

## 2. Vấn đề cần giải quyết
### Vấn đề chính
Game multiplayer luôn yêu cầu backend phức tạp gồm xác thực người chơi, đồng bộ dữ liệu, leaderboard tức thời và lưu trữ bền vững.  
Nếu triển khai theo mô hình server truyền thống:

- Chi phí máy chủ cao dù không có người chơi  
- Cần đội ngũ duy trì vận hành 24/7  
- Khó scale khi người chơi tăng đột biến  

Ngoài ra, hệ thống cần xử lý **avatar AI (biến đổi khuôn mặt/hình ảnh)** – tác vụ nặng, yêu cầu compute linh hoạt.

### Giải pháp đề xuất
Xây dựng **kiến trúc serverless hoàn toàn trên AWS**, gồm:

- Cognito quản lý đăng nhập & token identity  
- Lambda xử lý gameplay & nghiệp vụ  
- DynamoDB lưu state người chơi, kết hợp Streams để cập nhật realtime  
- S3 + Lambda Container xử lý avatar bằng OpenCV/MediaPipe  

### Lợi ích & Giá trị đầu tư (ROI)
| Giá trị | Mô tả |
|---|---|
| Chi phí thấp | Chỉ trả khi có người dùng tương tác |
| Mở rộng tự động | Tăng tải theo số lượng người chơi |
| Dễ triển khai | CI/CD push code là cập nhật ngay |
| Phát triển nhanh | BE/FE có thể làm song song, không phụ thuộc môi trường |

---

## 3. Kiến trúc giải pháp

Mô hình serverless theo hướng sự kiện (event-driven microservices).  
Unity gọi REST API khi gameplay diễn ra và kết nối WebSocket để nhận leaderboard realtime.  
Avatar được upload qua Presigned URL, xử lý bằng Lambda Container.

### Các dịch vụ AWS sử dụng

| Thành phần | Dịch vụ |
|---|---|
| Identity | Amazon Cognito |
| API Routing | API Gateway REST + WebSocket |
| Compute | Lambda Zip + Lambda Container |
| Database | DynamoDB + Streams |
| Storage | S3 Avatar Storage |
| AI Processing | ECR Container Image |
| CI/CD | CodePipeline + CodeBuild |

### Luồng dữ liệu hoạt động

1. Người dùng đăng nhập → Nhận JWT Token từ Cognito  
2. Unity → REST API → Lambda → DynamoDB ghi trạng thái  
3. Avatar Upload → Presigned URL → Lưu vào S3  
4. S3 Trigger / BE Trigger → Container Lambda xử lý ảnh  
5. Điểm Score thay đổi → DynamoDB Streams → WebSocket broadcast realtime  

---

## 4. Triển khai kỹ thuật

### Quy trình triển khai theo giai đoạn

| Giai đoạn | Mục tiêu |
|---|---|
| 1 – Setup hạ tầng | Cognito, DynamoDB, S3, API Gateway |
| 2 – Backend Core | Lambda API + container xử lý avatar |
| 3 – FE Authentication | AuthManager Unity + Signin UI |
| 4 – Kết nối hệ thống | API → Lambda → Streams realtime |
| 5 – Gameplay Integration | DataManager gọi REST/API WebSocket |
| 6 – E2E Testing | Login → Điểm → Leaderboard → Avatar |
| 7 – Deploy WebGL | Public WebGL build + domain redirect |

### Yêu cầu kỹ thuật

| Thành phần | Kỹ thuật yêu cầu |
|---|---|
| Frontend | Unity C#, AwsConfig, DataManager, WebSocket Client |
| Backend | Lambda Python/Node, Docker, Avatar AI |
| DevOps | IAM, VPC, API Gateway, CI/CD, CloudWatch |

---

## 5. Timeline & Milestones

| Giai đoạn | Thời gian | Kết quả |
|---|---|---|
| Phase 1 | Day 1–3 | Setup Cognito, S3, DynamoDB, API Gateway |
| Phase 2 | Day 3–8 | Lambda APIs + Avatar AI container |
| Phase 3 | Day 8–12 | FE tích hợp API + Streams realtime |
| Phase 4 | Day 13–15 | Test & Deploy WebGL chính thức |

---

## 6. Ước tính chi phí vận hành

| Dịch vụ | Chi phí dự kiến |
|---|---|
| Lambda | Hầu hết trong Free Tier |
| DynamoDB | Free Tier 25GB |
| S3 Storage | ~0.023 USD/GB |
| CloudWatch Logs | ~0.5–1 USD/tháng |
| ECR Storage | ~0.10 USD/GB |

**Tổng chi phí phát triển ước tính < 5 USD/tháng**  

---

## 7. Đánh giá rủi ro

| Rủi ro | Mức ảnh hưởng | Xác suất |
|---|---|---|
| Tích hợp phức tạp | Cao | Trung bình |
| Độ trễ khi load cao | Trung bình | Thấp |
| Chi phí phát sinh | Thấp | Thấp |

### Cách giảm thiểu

- FE dev bằng mock server khi API chưa sẵn  
- Thay thế leaderboard logic bằng tạm thời nếu BE delay  
- CloudWatch cảnh báo lỗi vượt 10 lần/phút  

---

## 8. Kết quả kỳ vọng

### Kỹ thuật đạt được

- Backend game full serverless  
- Login + dữ liệu người chơi an toàn  
- Leaderboard realtime bằng WebSocket  
- Avatar AI xử lý tự động bằng container Lambda

### Lợi ích dài hạn

- Có thể tái sử dụng cho game tiếp theo  
- Không cần quản lý server, tự mở rộng  
- Chi phí thấp, dễ bảo trì  

---
