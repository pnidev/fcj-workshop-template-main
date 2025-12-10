---
title: "Workshop"
date: "`r Sys.Date()`"
weight: 5
chapter: false
pre: " <b> 5. </b> "
---

{{% notice warning %}}
⚠️ **Lưu ý:** Các thông tin dưới đây chỉ nhằm mục đích tham khảo, vui lòng **không sao chép nguyên văn** cho bài báo cáo của bạn kể cả warning này.
{{% /notice %}}


# Đảm bảo truy cập Hybrid an toàn đến S3 bằng cách sử dụng VPC endpoint

#### Tổng quan

Workshop này hướng dẫn triển khai một hệ thống backend hoàn chỉnh cho game sử dụng các dịch vụ AWS.

Trong quá trình thực hành, bạn sẽ làm việc với:

- **Cognito** – xác thực tài khoản (Username/Password + Google OAuth)
- **DynamoDB** – lưu trữ hồ sơ người chơi, tiến độ và bảng điểm
- **S3** – lưu avatar người chơi thông qua upload bằng Presigned URL
- **Lambda (ZIP + Container)** – xử lý API backend + pipeline AI Avatar (OpenCV)
- **API Gateway REST / WebSocket** – cung cấp API + realtime leaderboard
- **CodeBuild + CodePipeline** – CI/CD build container & deploy tự động
- **IAM + CloudWatch** – phân quyền, log, giám sát, cảnh báo lỗi

#### Nội dung

1. [Tổng quan về workshop](5.1-Workshop-overview/)
2. [Chuẩn bị](5.2-Prerequiste/)
3. [Truy cập đến S3 từ VPC](5.3-S3-vpc/)
4. [Truy cập đến S3 từ TTDL On-premises](5.4-S3-onprem/)
5. [VPC Endpoint Policies (làm thêm)](5.5-Policy/)
6. [Dọn dẹp tài nguyên](5.6-Cleanup/)