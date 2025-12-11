---
title: "Workshop"
date: "`r Sys.Date()`"
weight: 5
chapter: false
pre: " <b> 5. </b> "
---

# Tổng quan Workshop: Triển khai Backend cho Game


## Mục tiêu chung
Workshop này hướng dẫn từng bước cách triển khai backend cho một dự án game (Unity/Web) trên AWS. Sau khi hoàn thành bạn sẽ có:


- Hệ thống đăng nhập (Cognito) với username/password và OAuth (Google).
- Cơ sở dữ liệu NoSQL dùng DynamoDB cho profile, tiến trình và điểm số.
- Lưu trữ avatar người chơi trên S3, upload thông qua presigned URL.
- Các hàm xử lý (Lambda) dạng ZIP cho logic nhẹ và Container Lambda (OpenCV/MediaPipe) cho xử lý ảnh/AI.
- API Gateway (REST) cho các route gameplay; API Gateway (WebSocket) cho realtime leaderboard.
- CI/CD (CodePipeline + CodeBuild) để build container, đẩy lên ECR và cập nhật Lambda.
- Cấu hình IAM, CloudWatch (log, báo lỗi, alarm) và tuỳ chọn WAF.


## Phần Lab (structure)
1. Cognito — xây hệ thống đăng nhập và authorizer JWT.
2. DynamoDB — tạo các bảng: `UserProfiles`, `UserProgress`, `Scores` (PK/SK tương ứng).
3. S3 — tạo bucket `game-avatars`, bật CORS, chỉ cho upload qua presigned URL.
4. ECR — repo để chứa image phục vụ Container Lambda (AvatarProcessing).
5. Lambda — tạo các function ZIP cho API và Container Lambda cho xử lý nặng.
6. API Gateway (REST) — khai báo route, map mỗi route tới Lambda, bật CORS.
7. API Gateway (WebSocket) — quản lý kết nối realtime cho leaderboard.
8. CI/CD — thiết lập CodePipeline + CodeBuild để tự động build & deploy.
9. IAM Roles — tạo role phù hợp cho các service (Lambda, CodeBuild, CodePipeline).
10. Logging & Monitoring — CloudWatch log group, alarm billing & error.
11. WAF — (tùy chọn) bảo vệ API.


## Chi tiết kỹ thuật quan trọng
### DynamoDB
- Bảng chính:
- `UserProfiles` (PK: `userId`)
- `UserProgress` (PK: `userId`)
- `Scores` (PK: `gameArea`, SK: `score`)
- Streams: `NEW_IMAGE` để Lambda/consumer xử lý event realtime.


### S3
- Bucket: `game-avatars`
- CORS: cho phép `PUT, POST, GET` và origin `*` (cấu hình production cần thu hẹp).
- Uploads thực hiện bằng presigned URL do backend cấp.


### API & Lambda
- REST endpoints tiêu biểu:
- `POST /score`, `GET /leaderboard`, `GET /leaderboard/global`
- `POST /progress`, `POST /unlock`, `POST /task/complete`
- `POST /money/add`, `POST /shop/buy`
- `POST /avatar/presign`, `POST /avatar/update`, `POST /avatar/process`
- WebSocket routes:
- `$connect`, `$disconnect`, `broadcast` — lưu/xóa `connectionId` trong DynamoDB.
- Lambda types:
- ZIP Lambdas: scoring, progress, shop, leaderboard.
- Container Lambda: `AvatarProcessingLambda` (OpenCV + MediaPipe) — deployed từ ECR image.


### CI/CD (CodePipeline + CodeBuild)
- Source: GitHub repo
- Build: CodeBuild (Docker enabled) → build image → tag → push ECR → update Lambda image.
- Ví dụ `buildspec.yml` đã được cung cấp để đăng nhập ECR, build & push image, gọi `aws lambda update-function-code`.

---

#### Nội dung

1. [Tổng quan về workshop](5.1-Workshop-overview/)
2. [Chuẩn bị](5.2-Prerequiste/)
3. [Tạo DynamoDB và S3 Bucket](5.3-DynamoDB-S3/)
4. [Tạo API Gateway và Lambda](5.4-API-GateWay-Lambda/)
5. [1 chút về CI/CD](5.5-CI-CD/)
6. [Dọn dẹp tài nguyên](5.6-Cleanup/)