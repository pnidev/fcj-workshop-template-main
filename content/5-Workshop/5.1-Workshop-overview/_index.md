---
title : "Workshop Overview"
date :  "`r Sys.Date()`" 
weight : 1 
chapter : false
pre : " <b> 5.1. </b> "
---

#### Workshop Objective

In this workshop, you will learn how to deploy a backend system for a game, including:

- **Cognito**: login system with Username/Password and OAuth (Google).
- **DynamoDB**: storing player profiles, progress, and scores.
- **S3**: storing player avatars, only allowing uploads via pre-signed URL.
- **Lambda**: backend APIs using standard ZIP Lambdas and Container Lambda (OpenCV).
- **API Gateway REST & WebSocket**: serving REST routes and realtime leaderboard.
- **CI/CD (CodePipeline + CodeBuild)**: automatically build container and deploy Lambda.
- **IAM Roles**: permissions for Lambda and CodeBuild.
- **CloudWatch**: basic logging, billing alarm, and error alarm.

You will deploy each AWS service step by step, test using CLI or console, and finally collect all necessary information to hand over to FE and BE teams.

#### Lab Sections

1. **Cognito – Login System**
2. **DynamoDB – Main Database**
3. **S3 – Store Player Avatars**
4. **ECR – Avatar Processing Container**
5. **Lambda – Backend APIs**
6. **API Gateway REST**
7. **API Gateway WebSocket**
8. **CI/CD – Backend**
9. **IAM Roles**
10. **Logging & Monitoring**
11. **WAF (Optional)**

#### Prerequisites

- AWS account with sufficient IAM permissions to create and configure the services above.
- Use **ap-southeast-2** region for this workshop.