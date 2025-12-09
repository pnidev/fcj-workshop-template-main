---
title: "Các bài blogs đã dịch"
date: "`r Sys.Date()`"
weight: 3
chapter: false
pre: " <b> 3. </b> "
---

{{% notice warning %}}  
⚠️ **Lưu ý:** Các thông tin dưới đây chỉ nhằm mục đích tham khảo, vui lòng **không sao chép nguyên văn** cho bài báo cáo của bạn kể cả warning này.
{{% /notice %}}

Tại đây sẽ là phần liệt kê, giới thiệu các blogs mà các bạn đã dịch. Ví dụ:

###  [Blog 1 - Getting started with healthcare data lakes: Using microservices](3.1-Blog1/)
Blog này giải thích cách cho dữ liệu trong Aurora PostgreSQL trở nên “hỏi được bằng ngôn ngữ tự nhiên” thông qua Amazon Bedrock Knowledge Bases. Tác giả dùng zero-ETL để tự động sao chép dữ liệu từ Aurora sang Amazon Redshift, sau đó Bedrock Knowledge Bases chuyển câu hỏi tiếng Anh thành SQL, chạy trên Redshift và trả lời lại dưới dạng ngôn ngữ tự nhiên. Bài viết đi qua các bước: tạo schema sample (product, customer, orders), cấu hình zero-ETL, tạo knowledge base và thử hỏi kiểu “Chúng ta có bao nhiêu khách hàng?”.
###  [Blog 2 - ...](3.2-Blog2/)
Blog này nói về cách dùng Powertools for AWS Lambda (TypeScript) với Parser utility mới để validate payload của các event (SQS, EventBridge, v.v.) một cách chuẩn và dễ bảo trì. Tác giả dùng thư viện Zod để định nghĩa schema (ví dụ orderSchema), sau đó parser sẽ parse + validate event, có thể tự động trích detail/body qua “envelope”, và hỗ trợ safeParse để bạn tự xử lý lỗi, log và đo lường (metrics). Bài cũng minh họa cách mở rộng schema có sẵn, thêm business rule phức tạp bằng .refine().
###  [Blog 3 - ...](3.3-Blog3/)
BBlog này mô tả một kiến trúc multi-tenant, multi-account cho các use case Generative AI dùng Amazon Bedrock. Tác giả thiết kế một hub account làm cổng vào chung (ALB, Cognito, routing, authz) và nhiều spoke account tách biệt cho từng tenant, kết nối bằng AWS Transit Gateway. Hub nhận request, xác thực, route tới Lambda của tenant, Lambda assume role sang spoke và gọi Bedrock trong VPC riêng, giúp vừa tập trung quản lý (auth, routing) vừa giữ được isolation, quota, chi phí và policy riêng cho từng tenant.
