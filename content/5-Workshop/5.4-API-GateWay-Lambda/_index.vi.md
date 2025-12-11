---
title : "Tổng Quan API & Lambda"
date :  "`r Sys.Date()`" 
weight : 4 
chapter : false
pre : " <b> 5.4. </b> "
---


### Tổng Quan API & Lambda

Mục này tóm tắt toàn bộ hệ thống API và dịch vụ xử lý backend cho dự án game.  
Hệ thống bao gồm:

- **REST API Gateway** – API chính để game giao tiếp với backend  
- **WebSocket API Gateway** – Cập nhật bảng xếp hạng theo thời gian thực  
- **AWS Lambda Functions** – Xử lý logic game và AI Avatar  

Dưới đây là phần mô tả tổng hợp của ba thành phần này.

---

## REST API Gateway – Lớp API chính của backend

REST API kết nối frontend (Unity/Web) với các Lambda function phía backend.

### Các endpoint chính

- `POST /score`
- `GET /leaderboard`
- `GET /leaderboard/global`
- `POST /progress`
- `POST /unlock`
- `POST /task/complete`
- `POST /money/add`
- `POST /shop/buy`
- `POST /avatar/presign`
- `POST /avatar/update`
- `POST /avatar/process` (gọi Lambda dạng container để xử lý avatar)

### Cấu hình

- Bật **CORS** cho tất cả route  
- Tạo **JWT Authorizer** kết nối với Amazon Cognito  
- Gán từng route với đúng Lambda function  

REST API đảm bảo giao tiếp bảo mật và rõ ràng giữa game và backend.

---

## WebSocket API – Bảng xếp hạng thời gian thực

WebSocket API dùng để cập nhật leaderboard theo thời gian thực mà không cần polling.

### Các route

- `$connect` → lưu `connectionId`
- `$disconnect` → xóa `connectionId`
- `broadcast` → gửi dữ liệu bảng xếp hạng cho tất cả client

### Cấu hình

- Lambda đọc danh sách `connectionId` từ DynamoDB  
- Phù hợp cho game cần cập nhật điểm liên tục  

Dịch vụ này giúp game hiển thị **bảng xếp hạng realtime**.

---

## Lambda Functions – Xử lý logic game

Hệ thống sử dụng hai loại Lambda:

### 1. Lambda dạng ZIP

Dùng cho các chức năng logic game tiêu chuẩn:

- điểm số  
- leaderboard  
- tiền tệ người chơi  
- lưu tiến trình  
- mở khóa nội dung  
- nhiệm vụ  

Mỗi API route có **một Lambda riêng** để tách biệt logic rõ ràng.

### 2. Lambda dạng Container

Dùng cho xử lý nặng, đặc biệt AI Avatar:

- OpenCV + MediaPipe  
- Tên gợi ý: `AvatarProcessingLambda`

Được deploy từ **image trên ECR**.  
DevOps tạo function, backend triển khai code xử lý.

---

### Tổng kết

Bạn đã thiết lập:

- **REST API** phục vụ các tác vụ backend  
- **WebSocket API** cho realtime leaderboard  
- Hệ thống **Lambda (ZIP + Container)** xử lý logic game và avatar AI  

Ba thành phần này tạo nên **hạ tầng backend cốt lõi** cho dự án game, giúp xử lý dữ liệu, giao tiếp real-time và tạo avatar thông minh.