---
title : "API Gateway rest"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 5.4.1 </b> "
---

#### REST API Gateway

Chúng ta sẽ tạo **REST API** để kết nối frontend với backend Lambda.

**Các route cần có:**

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
- `POST /avatar/process` ← gọi Lambda container để xử lý ảnh

**Cấu hình:**

- Bật **CORS** cho tất cả route.
- Tạo **JWT Authorizer** trỏ đến Cognito.
- Gán mỗi route vào đúng Lambda function.




