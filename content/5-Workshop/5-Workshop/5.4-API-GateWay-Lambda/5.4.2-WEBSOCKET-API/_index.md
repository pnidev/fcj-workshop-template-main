---
title : "Websocket API"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 5.4.2 </b> "
---

#### WebSocket API Gateway

WebSocket API is used for **real-time leaderboard**.

**Routes:**

- `$connect` → Lambda handler saves connectionId
- `$disconnect` → Lambda handler removes connectionId
- `broadcast` → Lambda handler sends leaderboard

**Configuration:**

- Lambda functions can read connectionIds from DynamoDB.