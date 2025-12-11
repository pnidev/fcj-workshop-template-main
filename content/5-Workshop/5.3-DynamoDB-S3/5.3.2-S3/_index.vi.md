---
title : "Tạo S3"
date :  "`r Sys.Date()`" 
weight : 2
chapter : false
pre : " <b> 5.3.2 </b> "
---

#### S3 – Lưu avatar người chơi

**Tạo bucket S3:**  
- Tên bucket: `game-avatars`  
- Bật **CORS**: cho phép PUT, POST, GET từ mọi origin `*`.  

**Lưu ý:** chỉ cho phép upload qua **presigned URL** do backend tạo.

---

#### Lời kết

Bạn đã tạo xong **3 bảng DynamoDB** và **bucket S3**. Các thông tin này sẽ được backend sử dụng để lưu trữ dữ liệu người chơi và avatar.