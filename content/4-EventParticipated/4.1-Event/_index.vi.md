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
## 1. Tóm tắt điều hành (Executive Summary)
Mục tiêu của dự án là xây dựng một hạ tầng backend serverless vững chắc cho game multiplayer viết bằng Unity. Hệ thống tách rõ trách nhiệm giữa ba nhóm: DevOps, Frontend (Unity) và Backend.  
Các dịch vụ AWS được sử dụng để xử lý:

- **Xác thực người dùng (User Authentication)** → Amazon Cognito  
- **Logic gameplay** → AWS Lambda  
- **Leaderboard thời gian thực** → API Gateway WebSocket + DynamoDB Streams  
- **Xử lý AI Avatar** → Lambda chạy bằng container (OpenCV/MediaPipe)  

Kiến trúc này cung cấp tính sẵn sàng cao (high availability), tự động hóa CI/CD, và tích hợp WebGL mượt mà được deploy lên itch.io hoặc CloudFront.

---
## 2. Vấn đề cần giải quyết (Problem Statement)
### Vấn đề là gì?
Game multiplayer cần nhiều tính năng backend phức tạp như: xác thực, giao tiếp thời gian thực, và lưu trữ/persist dữ liệu. Các server monolithic truyền thống thường tốn kém, khó bảo trì, và không scale tốt khi số lượng người chơi tăng đột biến.  

Game này còn cần **xử lý AI cho việc biến đổi avatar**, vốn rất tốn tài nguyên tính toán.

### Giải pháp
Một kiến trúc AWS hoàn toàn **serverless**:

- **Authentication:** Amazon Cognito (User Pools + Hosted UI)  
- **Logic & Compute:** Lambda (zip + container images từ ECR)  
- **Tương tác thời gian thực:** WebSocket API + DynamoDB Streams  
- **Lưu trữ:** Amazon S3 cho avatar/asset  

### Lợi ích & ROI
- **Tiết kiệm chi phí:** Trả theo mức sử dụng (Lambda + DynamoDB)  
- **Khả năng mở rộng:** Tự động scale khi có spike người chơi  
- **Tự động hóa:** Pipeline CI/CD cho deploy nhanh, ít thao tác tay  

---
## 3. Kiến trúc giải pháp (Solution Architecture)
Hệ thống tuân theo mô hình microservices hướng sự kiện (event-driven). Unity tương tác với AWS qua REST (điểm, shop) và WebSocket (leaderboard realtime). Xử lý AI được thực hiện bởi các Lambda dùng container.

### Các dịch vụ AWS sử dụng
- **Amazon Cognito** – User Pools, Hosted UI  
- **API Gateway (REST + WebSocket)**  
- **AWS Lambda (Zip + Container Image)**  
- **Amazon DynamoDB + Streams**  
- **Amazon S3**  
- **Amazon ECR**  
- **CodePipeline & CodeBuild**  

### Thiết kế các thành phần (Component Design)
#### Frontend
Build Unity WebGL được host trên itch.io hoặc CloudFront.

#### Luồng dữ liệu (Data Flow)
1. Người dùng đăng nhập → nhận Cognito Token  
2. Unity gọi REST API → Lambda → DynamoDB  
3. Upload Avatar → Presigned URL → S3 → Lambda (AI Container)  
4. Cập nhật điểm (Score updates) → DynamoDB Stream → broadcast qua WebSocket  

---
## 4. Triển khai kỹ thuật (Technical Implementation)
### Các giai đoạn triển khai (Implementation Phases)
1. **Thiết lập hạ tầng (DevOps)** – Cognito, DynamoDB, S3, API Gateway  
2. **Backend Skeleton (BE)** – Đặc tả API, Postman collection, code Lambda cơ bản  
3. **Tích hợp đăng nhập (FE)** – Unity AuthManager  
4. **Kết nối & Streams (DevOps)** – Nối API → Lambda, bật Streams  
5. **Gameplay Integration (FE)** – DataManager → gọi REST APIs  
6. **Kiểm thử End-to-End** – Login, Shop, Leaderboards, Avatar  
7. **Triển khai (Deployment)** – Build WebGL + cấu hình redirect URLs  

### Yêu cầu kỹ thuật (Technical Requirements)
- **Frontend:** Unity (C#) – AwsConfig, AuthManager, DataManager, RealtimeManager  
- **Backend:** Node.js/Python cho các Lambda, Docker cho AI containers  
- **DevOps:** IAM roles, CloudFormation (tùy chọn), WAF (tùy chọn)  

---
## 5. Timeline & Milestones
### Giai đoạn 1: Nền tảng (Days 1–3)
- DevOps thiết lập Cognito, DynamoDB, S3, API Gateway.

### Giai đoạn 2: Phát triển logic (Days 3–8)
- Backend xây dựng các Lambda functions + AI Container xử lý Avatar  
- Frontend xây dựng chức năng Login

### Giai đoạn 3: Tích hợp (Days 8–12)
- DevOps kết nối Streams  
- Frontend tích hợp các API

### Giai đoạn 4: Kiểm thử & Launch (Days 13–15)
- Kiểm thử leaderboard realtime + pipeline avatar  
- Deploy bản WebGL

---
## 6. Ước tính chi phí (Budget Estimation)
*(Tham khảo AWS Pricing Calculator)*

### Chi phí hạ tầng (Infrastructure Costs)
- **Lambda:** Chủ yếu nằm trong Free Tier  
- **DynamoDB:** Free Tier (25GB)  
- **S3:** ~0,023 USD/GB  
- **CloudWatch:** ~0,5–1 USD/tháng  
- **ECR:** ~0,10 USD/GB  

**Tổng chi phí ước tính:** **< 5 USD/tháng** trong giai đoạn phát triển.

---
## 7. Đánh giá rủi ro (Risk Assessment)
### Ma trận rủi ro (Risk Matrix)
- **Độ phức tạp tích hợp:** Ảnh hưởng cao / Xác suất trung bình  
- **Vấn đề độ trễ (latency):** Ảnh hưởng trung bình / Xác suất thấp  
- **Chi phí phát sinh không mong muốn:** Ảnh hưởng thấp / Xác suất thấp  

### Chiến lược giảm thiểu (Mitigation Strategies)
- Dùng Postman Mock Server để FE có thể phát triển song song  
- Dùng logic placeholder cho leaderboard/task khi BE chưa xong  
- CloudWatch Logs + alarms (ví dụ lỗi > 10/min)  

---
## 8. Kết quả kỳ vọng (Expected Outcomes)
### Cải tiến kỹ thuật
- Backend cho game hoàn toàn serverless  
- Bảo mật danh tính và dữ liệu người chơi  
- Leaderboard thời gian thực  
- Xử lý AI avatar tự động trên nền tảng Lambda/container  

### Giá trị dài hạn (Long-term Value)
- Kiến trúc tái sử dụng cho các game tương lai  
- Tự động scale mà không cần quản lý server  
- Chi phí vận hành thấp
---
