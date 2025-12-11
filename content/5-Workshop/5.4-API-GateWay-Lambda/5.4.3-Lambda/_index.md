---
title : "Lambda"
date : "`r Sys.Date()`"
weight : 3
chapter : false
pre : " <b> 5.4.3 </b> "
---

#### Lambda Functions

Two types:

1. **Standard Lambda (ZIP)**  
 - Used for: `score`, `leaderboard`, `money`, `progress`, `unlock`, `task`.
 - Create one Lambda per API route.

2. **Lambda container**  
 - Used for avatar AI (OpenCV + MediaPipe).
 - Suggested name: `AvatarProcessingLambda`
 - Creation: Lambda → Create Function → Container Image → choose image from ECR.
 - DevOps only creates the function; backend implements the code.

---

#### Summary

You have created:

- REST API for the backend
- WebSocket API for real-time leaderboard
- Lambda functions (ZIP + Container)  

All these details are provided to the backend for connection and deployment.




