---
title: "Workshop"
date: "`r Sys.Date()`"
weight: 5
chapter: false
pre: " <b> 5. </b> "
---
{{% notice warning %}}
⚠️ **Note:** The information below is for reference purposes only. Please **do not copy verbatim** for your report, including this warning.
{{% /notice %}}

# Secure Hybrid Access to S3 using VPC Endpoints

#### Overview

This workshop guides you through deploying a complete backend system for a cloud-based game, using AWS managed services.

You will work with:

- **Cognito** for authentication (Username/Password + OAuth Google)
- **DynamoDB** for user progress, profile, scores
- **S3** for storing avatars via presigned URL upload
- **Lambda (ZIP + Container)** for backend logic and avatar processing (OpenCV)
- **API Gateway REST / WebSocket** for API routing and realtime leaderboard
- **CodePipeline + CodeBuild** for CI/CD deployment
- **IAM Roles & CloudWatch** for security, logging & monitoring

The workshop includes step-by-step deployment and a final clean-up to ensure no unused resources incur cost.

---

#### Content

1. [Workshop overview](5.1-Workshop-overview)
2. [Prerequiste](5.2-Prerequiste/)
3. [Access S3 from VPC](5.3-S3-vpc/)
4. [Access S3 from On-premises](5.4-S3-onprem/)
5. [VPC Endpoint Policies (Bonus)](5.5-Policy/)
6. [Clean up](5.6-Cleanup/)