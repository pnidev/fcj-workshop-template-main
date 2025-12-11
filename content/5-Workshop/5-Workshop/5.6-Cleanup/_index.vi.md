---
title : "Dọn dẹp tài nguyên"
date : "`r Sys.Date()`"
weight : 6
chapter : false
pre : " <b> 5.6. </b> "
---

# 5.6 Dọn dẹp môi trường Workshop

Sau khi hoàn thành workshop, hãy dọn dẹp môi trường để tránh phát sinh chi phí không cần thiết trên AWS.

---

## Xóa các Lambda Functions

1. Vào **AWS Lambda Console**.
2. Chọn tất cả các Lambda Functions bạn đã tạo:
   - AvatarProcessingLambda
   - BroadcastHandler
   - ConnectHandler
   - DisconnectHandler
   - LeaderboardFunction
   - MoneyFunction
   - ProgressFunction
   - ScoreFunction
   - TaskFunction
   - UnlockFunction
3. Chọn **Actions → Delete** và xác nhận.

---

## Xóa API Gateway

1. Vào **API Gateway Console**.
2. Chọn REST API và WebSocket API của workshop.
3. Chọn **Actions → Delete API** và xác nhận.

---

## Xóa S3 Buckets

1. Vào **S3 Console**.
2. Xóa tất cả các bucket đã tạo:
   - game-avatars
   - Các bucket phục vụ workshop (bucket-1, bucket-2,…)
3. Lưu ý xóa toàn bộ nội dung bên trong trước khi xóa bucket.

---

## Xóa DynamoDB Tables

1. Vào **DynamoDB Console**.
2. Xóa các bảng:
   - UserProfiles
   - UserProgress
   - Scores
3. Xác nhận xóa từng bảng.

---

## Xóa IAM Roles

1. Vào **IAM Console → Roles**.
2. Xóa các Role đã tạo cho workshop:
   - Lambda Execution Role
   - CodeBuild Role
3. Đảm bảo không còn Policy nào gắn với Role trước khi xóa.

---

## Xóa ECR Repositories

1. Vào **ECR Console**.
2. Xóa repository `avatar-processing`.
3. Xác nhận xóa toàn bộ images trong repository.

---

## Xóa CodePipeline / CodeBuild

1. Vào **CodePipeline Console**.
2. Xóa pipeline workshop.
3. Xóa project CodeBuild tương ứng.

---

## Kiểm tra chi phí

1. Truy cập **AWS Billing Console → Cost Explorer**.
2. Đảm bảo không còn dịch vụ nào phát sinh chi phí từ workshop.
3. Nếu còn dịch vụ nào tồn tại, xóa theo hướng dẫn tương ứng.

---

## Tóm tắt

- Môi trường workshop đã được dọn dẹp hoàn toàn.
- Không còn Lambda, API, S3, DynamoDB, IAM Role, ECR, hay pipeline nào tồn tại.
- Chi phí phát sinh từ workshop đã được giảm về 0.