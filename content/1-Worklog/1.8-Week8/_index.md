---
title: "Week 8 Worklog"
date: 2025-10-28
weight: 8
chapter: false
pre: " <b> 1.8. </b> "
---

### Week 8 Objectives:

* Learn how **Amazon Route 53** handles DNS management in cloud environments.
* Explore **Amazon CloudFront** — a CDN service for global content acceleration.
* Understand the role of **Lambda@Edge** in processing logic at edge locations.
* Build a low-latency global content delivery architecture with reliability and scalability.

---

### Weekly Tasks Completed:

| Day | Task                                                                                                                | Start       | Completed       | Reference Docs                                |
|---|-----------------------------------------------------------------------------------------------------------------------|-------------|-----------------|----------------------------------------------|
| 1  | - Overview of **Route 53** <br> - Review DNS concepts: A, CNAME, Alias <br> - Create hosted zone and basic records     | 28/10/2025  | 28/10/2025      | https://000010.awsstudygroup.com/vi/        |
| 2  | - Apply routing policies: Simple, Weighted, Failover <br> - Configure Health Check for failover automation            | 29/10/2025  | 29/10/2025      | https://000010.awsstudygroup.com/vi/2-prerequiste/ |
| 3  | - Introduction to **CloudFront CDN** <br> - Study edge locations and cache TTL concepts <br> - Create first distribution | 30/10/2025  | 30/10/2025      | https://000094.awsstudygroup.com/vi/        |
| 4  | - Connect CloudFront with **S3 origin** <br> - Map custom domain using Route 53 <br> - Generate SSL certificate via ACM | 31/10/2025  | 31/10/2025      | https://000094.awsstudygroup.com/vi/1.-cloud-front-với-s3/ |
| 5  | - Learn how **Lambda@Edge** works <br> - Deploy CloudFront + Custom Domain + SSL <br> - Implement request/response logic at edge <br> - Monitor CDN performance and latency | 01/11/2025 | 01/11/2025      | https://000130.awsstudygroup.com/vi/       |

---

### Week 8 Achievements:

* Gained a solid understanding of DNS management using Route 53.
* Successfully applied routing policies for failover and traffic distribution.
* Built a functional CloudFront CDN to deliver content globally.
* Used Lambda@Edge to process logic directly at edge networks.
* Integrated Route 53 + CloudFront + ACM SSL + Edge computing into a full CDN pipeline.

---

### Summary:

By the end of Week 8, I was able to:

* Optimize global content delivery using Route 53 and CloudFront.
* Issue SSL/TLS certificates via ACM and apply them to custom domains.
* Customize CloudFront behavior using Lambda@Edge request/response logic.
* Reduce latency for users across multiple regions with a global CDN approach.
* Understand how to scale applications to global-level performance.

---
