---
title : "Websocket API"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 5.4.2 </b> "
---

#### WebSocket API Gateway

WebSocket API dùng cho **real-time leaderboard**.

**Các route:**

- `$connect` → Lambda handler lưu connectionId
- `$disconnect` → Lambda handler xóa connectionId
- `broadcast` → Lambda handler gửi leaderboard

**Cấu hình:**

- Cho phép Lambda đọc connectionId trong DynamoDB. 