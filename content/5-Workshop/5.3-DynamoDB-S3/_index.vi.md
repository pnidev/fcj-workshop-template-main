---
title : "Tóm tắt về DynamoDB và S3"
date :  "`r Sys.Date()`" 
weight : 3
chapter : false
pre : " <b> 5.3. </b> "
---

### Database & Storage Summary

Trong dự án game, chúng ta sử dụng **AWS DynamoDB** và **AWS S3** để lưu trữ thông tin người chơi và avatar.

---

## DynamoDB – Main Database

DynamoDB được dùng để lưu:

- Thông tin hồ sơ người chơi  
- Tiến trình chơi  
- Điểm số theo từng khu vực game  

### Các bảng được tạo

1. UserProfiles
   - Partition Key (PK): `userId`

2. UserProgress
   - Partition Key (PK): `userId`

3. Scores
   - Partition Key (PK): `gameArea`
   - Sort Key (SK): `score`

### DynamoDB Streams

- Bật **Stream type: NEW IMAGE**
- DevOps cung cấp **Stream ARN** cho backend để Lambda có thể xử lý sự kiện.

---

## S3 – Store Player Avatars

### Tạo S3 Bucket

- Bucket name: `game-avatars`
- Bật **CORS**:
  - Cho phép `PUT`, `POST`, `GET`
  - Origin: `*`

### Lưu ý

- Chỉ cho phép upload thông qua **presigned URL** do backend sinh ra.

---

### Tổng kết

Bạn đã thiết lập:

- **3 bảng DynamoDB**  
- **1 S3 bucket** để lưu avatar người chơi  

Những tài nguyên này sẽ được backend sử dụng để lưu trữ dữ liệu người chơi một cách an toàn và hiệu quả.

#### Nội dung

- [Tạo DynamoDB](5.3.1-DynamoDB/)
- [Tạo S3 Bucket](5.3.2-S3/)