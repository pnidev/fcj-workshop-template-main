---
title: "Week 7 Worklog"
date: 2025-10-21
weight: 7
chapter: false
pre: " <b> 1.7. </b> "
---

### Week 7 Objectives:

* Explore how **EC2 Auto Scaling** maintains application performance during traffic spikes or drop-offs.
* Create and configure **Auto Scaling Groups** using custom Launch Templates.
* Get hands-on experience with **Amazon CloudWatch** for real-time resource monitoring.
* Implement **automated scaling policies** based on system metrics.

---

### Weekly Tasks Completed:

| Day | Task                                                                                                        | Start       | Completed       | Reference Docs                                       |
|---|---------------------------------------------------------------------------------------------------------------|-------------|-----------------|------------------------------------------------------|
| 1  | - Study the core concept of **Auto Scaling** <br> - Compare scaling policies: Target Tracking / Step / Scheduled <br> - Review Launch Template fundamentals | 21/10/2025 | 21/10/2025 | https://000006.awsstudygroup.com/vi/                |
| 2  | - Create custom **Launch Template** <br> - Deploy **Auto Scaling Group** in VPC <br> - Configure Min–Desired–Max capacity parameters | 22/10/2025 | 22/10/2025 | https://000006.awsstudygroup.com/vi/6-create-auto-scaling-group/ |
| 3  | - Learn **CloudWatch Metrics & Namespaces** <br> - Build monitoring Dashboard for EC2 activities              | 23/10/2025 | 23/10/2025 | https://000008.awsstudygroup.com/vi/                |
| 4  | - Create **CloudWatch Alarm** based on CPU threshold <br> - Enable SNS email notifications <br> - Bind alarm to scaling policies | 24/10/2025 | 24/10/2025 | https://000008.awsstudygroup.com/vi/5-cloud-watch-alarm/ |
| 5  | - **Hands-on Lab:** <br> + Deploy Auto Scaling Group + Scaling Policies <br> + Load test to trigger scaling events <br> + Observe scaling logs in CloudWatch <br> + Release resources after test completion | 25/10/2025 | 25/10/2025 | FCJ Training Notes, Monitoring Best Practices |

---

### Week 7 Achievements:

* Gained a deeper understanding of how **EC2 Auto Scaling** functions under variable workload.
* Successfully created and configured **Launch Template + Auto Scaling Group** for dynamic scaling.
* Built CloudWatch dashboards to track **CPU, Network metrics, and system health checks**.
* Setup automatic alerting using **CloudWatch Alarm + SNS Notification**.
* Simulated load and observed scale-out/scale-in behavior in real-time.

---

### Summary:

By the end of Week 7, I was able to:

* Deploy an Auto Scaling environment that maintains stable application performance.
* Select **suitable scaling policies** based on workload patterns and thresholds.
* Monitor instance metrics using CloudWatch dashboards and respond via alert triggers.
* Conduct load testing to validate automatic scaling behavior.
* Optimize EC2 resource usage to balance uptime reliability and cost efficiency.

---
