---
title: "Proposal"
date: "`r Sys.Date()`"
weight: 2
chapter: false
pre: " <b> 2. </b> "
---
# Serverless Multiplayer Game Backend  
A Scalable AWS Solution for Real-Time Gaming & AI Avatar Processing

---

## 1. Executive Summary

This proposal outlines the development of a **fully serverless backend infrastructure** for a multiplayer Unity game.  
The system is divided into three main responsibilities: DevOps, Backend, and Unity Frontend.

AWS services will be used to provide:

- **User Authentication** → Amazon Cognito  
- **Gameplay & Business Logic** → AWS Lambda  
- **Real-time Leaderboards** → API Gateway WebSocket + DynamoDB Streams  
- **AI Avatar Generation** → Lambda Container (OpenCV/MediaPipe)

This architecture ensures **scalability, fault tolerance, CI/CD automation**, and supports seamless deployment for WebGL via **itch.io or CloudFront**.

---

## 2. Problem Statement

### What’s the Problem?

Multiplayer games require real-time backend capability, identity management, and persistent state storage.  
Traditional server-based systems:

- are expensive to run long-term  
- scale poorly under heavy traffic spikes  
- increase maintenance overhead  

Additionally, this project requires **AI avatar transformation**, which demands high compute power.

### The Solution

A scalable serverless architecture using AWS services:

- **Authentication:** Amazon Cognito (User Pools + Hosted UI)
- **Logic:** AWS Lambda (Zip + ECR container images)
- **Storage:** S3 for avatars + DynamoDB for player metadata
- **Realtime:** WebSocket API + DynamoDB Streams

### Benefits & ROI

- **Highly Cost Efficient** → Pay-per-execution, no idle servers  
- **Auto Scaling** → Handles traffic bursts without manual scaling  
- **CI/CD Ready** → Quick feature rollout through CodePipeline/CodeBuild  

---

## 3. Solution Architecture

The system uses an event-driven microservice approach.  
Unity communicates with backend via **REST APIs** and **WebSocket live streams**, while avatar processing runs via container-based Lambda.
![Game Architecture](/images/AWS.png)

### AWS Services Used

| Category | Services |
|---|---|
| Identity | Amazon Cognito |
| API | API Gateway REST + WebSocket |
| Compute | AWS Lambda (Zip + Container) |
| Database | DynamoDB + Streams |
| Storage | S3 |
| Image Processing | ECR Container Lambda |
| CI/CD | CodePipeline, CodeBuild |

### Component Design

#### Frontend

Unity WebGL build deployed on **itch.io / CloudFront**.

#### Data Flow

1. User Login → Cognito Issues Token  
2. Unity → REST API (Lambda → DynamoDB)  
3. Avatar Upload → Presigned URL → S3  
4. AI Avatar Processor → Lambda Container (OpenCV/MediaPipe)  
5. Score Updates → DynamoDB Stream → WebSocket Broadcast Live  

---

## 4. Technical Implementation

### Implementation Phases

1. **Infrastructure Setup (DevOps)**  
   Cognito, DynamoDB, S3, API Gateway

2. **Backend Skeleton (BE)**  
   Lambda endpoints + Postman scripts + API routing

3. **Frontend Login (FE)**  
   AuthManager + Cognito integration

4. **Integration Wiring (DevOps)**  
   REST + WebSocket + DynamoDB Streams wiring

5. **Gameplay API Integration (FE)**  
   DataManager for score, money, progress, tasks

6. **End-to-End Testing**  
   Login → Game API → Leaderboard → Avatar

7. **Deployment**  
   Unity WebGL build + Allowed Redirect URLs

### Tech Stack Requirements

| Role | Stack |
|---|---|
| Frontend | Unity (C#), AwsConfig, AuthManager, DataManager, RealtimeManager |
| Backend | Lambda NodeJS/Python, Docker Container (OpenCV/MediaPipe) |
| DevOps | IAM, API Gateway, DynamoDB Streams, CodePipeline/Build |

---

## 5. Timeline & Milestones

| Phase | Duration | Deliverables |
|---|---|---|
| Foundation | Days 1–3 | Cognito, DynamoDB, S3, API Gateway |
| Logic Development | Days 3–8 | Lambda API + Avatar Container + Unity Login |
| Integration | Days 8–12 | Streams wired, APIs connected |
| Testing & Launch | Days 13–15 | WebGL live deployment + Leaderboard & Avatar tests |

---

## 6. Budget Estimation  
*(Based on AWS Pricing Calculator)*

| Resource | Cost |
|---|---|
| Lambda | Free Tier (low execution) |
| DynamoDB | Free Tier (25GB) |
| S3 Storage | $0.023/GB |
| CloudWatch Logs | ~$0.5 - $1/month |
| ECR Storage | ~$0.10/GB |

**Estimated Total Cost < $5/month during development.**

---

## 7. Risk Assessment

### Risk Matrix

| Risk | Impact | Probability |
|---|---|---|
| Integration complexity | High | Medium |
| Latency during peak loads | Medium | Low |
| Cost overrun | Low | Low |

### Mitigation Strategy

- Mock APIs using Postman during FE development  
- Placeholder logic for shop & leaderboard  
- CloudWatch alarm triggers on error spikes (>10/min)  

---

## 8. Expected Outcomes

### Technical Outcomes

- Fully serverless backend infrastructure  
- Secure identity & data persistence  
- Real-time leaderboard support  
- Automated avatar processing pipeline  

### Long-Term Value

- Reusable architecture for any future game titles  
- Auto-scales without provisioning servers  
- Extremely low operational cost  

---
