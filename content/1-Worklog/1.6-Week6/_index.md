---
title: "Week 6 Worklog"
date: 2025-10-14
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
---

### Week 6 Objectives:

* Gain familiarity with **Amazon ElastiCache** and understand cache usage in distributed systems.
* Learn how Redis/Memcached improves query performance through caching.
* Integrate ElastiCache with services used in previous weeks for real-world application flow.
* Review RDS + DynamoDB knowledge to build a performance-focused architecture using cache.

---

### Weekly Tasks Completed:

| Day | Task                                                                                                          | Start       | Completed       | References                                                         |
|---|---------------------------------------------------------------------------------------------------------------|-------------|-----------------|--------------------------------------------------------------------|
| 1  | - Overview of **ElastiCache** <br> - Difference between Redis vs Memcached <br> - Learn cache-first & cache-aside patterns | 14/10/2025 | 14/10/2025 | https://000061.awsstudygroup.com/vi/1-introduce/                  |
| 2  | - Create **Redis Cluster** in ElastiCache <br> - Configure VPC Security Groups <br> - Test connection from EC2       | 15/10/2025 | 15/10/2025 | https://000061.awsstudygroup.com/vi/3-amazonelasticacheforredis/3.4-grantaccesstocluster/ |
| 3  | - Implement caching layer for application <br> - Apply TTL & manual invalidation <br> - Monitor cache hit/miss ratio | 16/10/2025 | 16/10/2025 | AWS Caching Guidelines, FCJ Notes                                  |
| 4  | - **Integration Lab:** <br>  + Connect RDS + Redis <br>  + Store sessions & cache query results <br>  + Measure response performance | 17/10/2025 | 17/10/2025 | AWS Architecture Labs                                              |
| 5  | - **Mini Project:** <br>  + Deploy full 3-tier architecture (EC2 → Redis → RDS) <br>  + Benchmark with & without cache <br>  + Optimize DB architecture & cost <br>  + Clean up resources to avoid charges | 18/10/2025 | 18/10/2025 | Well-Architected Framework, FCJ Session |

---

### Week 6 Achievements:

* Understood the role of **In-Memory Cache** and why Redis is essential in large-scale systems.
* Clearly differentiated **Redis** (session/state caching) vs **Memcached** (simple key-value memory store).
* Successfully built a **Caching Layer** to reduce DB query load.
* Integrated ElastiCache + RDS to significantly improve query response speed.
* Completed a scalable web architecture demo with improved performance.

---

### Summary:

By the end of Week 6, I was able to:

* Create and connect to an ElastiCache Redis cluster in a private VPC.
* Design cache logic for frequently accessed data and API responses.
* Integrate Redis to store session/state values and cache database queries.
* Benchmark performance gains and evaluate cache benefit on system load.
* Understand how **RDS + DynamoDB + Redis** complement each other in real-world system design.

---
