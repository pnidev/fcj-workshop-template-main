---
title : "Create DynamoDB"
date :  "`r Sys.Date()`" 
weight : 1
chapter : false
pre : " <b> 5.3.1 </b> "
---

#### DynamoDB – Main Database

In the game project, we use **DynamoDB** to store player profiles, progress, and scores.

**Create 3 tables:**

1. **UserProfiles**
   - Partition Key (PK): `userId`
2. **UserProgress**
   - Partition Key (PK): `userId`
3. **Scores**
   - Partition Key (PK): `gameArea`
   - Sort Key (SK): `score`

**Enable DynamoDB Stream**  
- Stream type: **NEW IMAGE**  
- DevOps provides the **Stream ARN** to the backend so Lambda can consume events.