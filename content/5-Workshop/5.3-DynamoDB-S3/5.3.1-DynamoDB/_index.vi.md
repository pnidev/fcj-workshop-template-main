---
title : "Tạo DynamoDB"
date :  "`r Sys.Date()`" 
weight : 1
chapter : false
pre : " <b> 5.3.1 </b> "
---

#### DynamoDB – Database chính

Trong dự án game, chúng ta sẽ dùng **DynamoDB** để lưu trữ thông tin người chơi, tiến trình chơi và điểm số.

**Tạo 3 bảng sau:**

1. **UserProfiles**
   - Partition Key (PK): `userId`
2. **UserProgress**
   - Partition Key (PK): `userId`
3. **Scores**
   - Partition Key (PK): `gameArea`
   - Sort Key (SK): `score`

**Bật DynamoDB Stream**  
- Stream type: **NEW IMAGE**  
- DevOps cung cấp cho backend **Stream ARN** để Lambda có thể đọc sự kiện.
