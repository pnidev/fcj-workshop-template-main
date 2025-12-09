---
title: "Week 5 Worklog"
date: 2025-10-07
weight: 5
chapter: false
pre: " <b> 1.5. </b> "
---

### Week 5 Objectives:

* Understand how **Amazon RDS** operates and when relational databases are the right choice.
* Deploy, configure, and test-connect to **RDS instances** inside a VPC environment.
* Explore **Amazon DynamoDB** — a fully managed NoSQL service built for high performance and scalability.
* Compare RDS vs DynamoDB to identify which database model suits different workloads.

---

### Weekly Tasks Completed:

| Day | Task                                                                                                                         | Start       | Completed       | References                                                |
|---|--------------------------------------------------------------------------------------------------------------------------------|-------------|-----------------|-----------------------------------------------------------|
| 1  | - Overview of **Amazon RDS** <br> - Learn MySQL/PostgreSQL engines <br> - Study Multi-AZ replication & Read Replica behavior   | 07/10/2025  | 07/10/2025      | https://000005.awsstudygroup.com/vi/                     |
| 2  | - Create first RDS instance <br> - Configure Security Groups for DB access <br> - Connect through EC2 + MySQL Workbench/CLI    | 08/10/2025  | 08/10/2025      | https://000005.awsstudygroup.com/vi/4-create-rds/        |
| 3  | - Learn snapshots & automated backups <br> - Restore DB from snapshot <br> - Test data recovery scenarios                     | 09/10/2025  | 09/10/2025      | https://000005.awsstudygroup.com/vi/6-backup/            |
| 4  | - Introduction to **DynamoDB** <br> - Understand NoSQL model, PK/SK schema design <br> - Create table and run queries         | 10/10/2025  | 10/10/2025      | https://000060.awsstudygroup.com/vi/                     |
| 5  | - **Mini Lab:** <br> + Create MySQL RDS DB + CRUD operations <br> + Create DynamoDB table & read/write data <br> + Benchmark RDS vs DynamoDB performance <br> + Resource cleanup | 11/10/2025 | 11/10/2025 | https://000005.awsstudygroup.com/vi/5-deploy-app/       |

---

### Week 5 Achievements:

* Gained solid understanding of RDS operation, replication, and backup mechanisms.
* Successfully created, configured, and queried RDS from EC2.
* Performed DB restoration using automated snapshots.
* Created DynamoDB tables and executed CRUD operations via Console/SDK.
* Built practical comparison insight between relational vs NoSQL performance.

---

### Summary:

By the end of Week 5, I achieved the following:

* Fully operated an RDS MySQL instance — from creation to backup & recovery.
* Implemented DB access control using Security Groups safely.
* Understood snapshot/restore for failure recovery and rollback scenarios.
* Designed DynamoDB schema for high-traffic workloads.
* Identified use cases where RDS is stronger for transactions, while DynamoDB excels in large-scale, low-latency distributed applications.

---
