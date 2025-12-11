---
title : "API & Lambda Overview"
date : "`r Sys.Date()`"
weight : 4
chapter : false
pre : " <b> 5.4. </b> "
---

### API & Lambda Overview

This section summarizes all API and compute services used in the game backend.  
The system includes:

- **REST API Gateway** – Main API for gameplay actions  
- **WebSocket API Gateway** – Real-time leaderboard communication  
- **AWS Lambda Functions** – Backend compute for all game logic and avatar processing  

Below is a combined overview of how these services operate together.

---

## REST API Gateway – Backend API Layer

The REST API connects the frontend (Unity/Web) to the backend Lambda functions.

### Main Endpoints

- `POST /score`
- `GET /leaderboard`
- `GET /leaderboard/global`
- `POST /progress`
- `POST /unlock`
- `POST /task/complete`
- `POST /money/add`
- `POST /shop/buy`
- `POST /avatar/presign`
- `POST /avatar/update`
- `POST /avatar/process` (proxy to the avatar container Lambda)

### Configuration

- Enable **CORS** for all routes  
- Use **JWT Authorizer** connected to Amazon Cognito  
- Each route is mapped to its own Lambda handler  

REST API ensures secure and structured communication for all core game features.

---

## WebSocket API – Real-Time Leaderboard

The WebSocket API handles live leaderboard updates.

### Routes

- `$connect` → store `connectionId`
- `$disconnect` → remove `connectionId`
- `broadcast` → push updated leaderboard to all players

### Configuration

- Lambda functions read active connectionIds from DynamoDB  
- Ideal for timely score updates without polling  

This service allows the game to display **real-time ranking updates**.

---

## Lambda Functions – Game Logic Execution

Two Lambda types are used:

### 1. Standard Lambda (ZIP)

Used for standard game logic:

- scoring  
- leaderboard  
- player money  
- progress saving  
- unlocking features  
- task system  

Each API route has a **dedicated Lambda function** to keep logic clean and modular.

### 2. Container Lambda

Used for heavy AI processing:

- Avatar generation (OpenCV + MediaPipe)
- Function name suggestion: `AvatarProcessingLambda`

Deployed from an **ECR container image**.  
DevOps prepares the function, and backend developers implement the code.

---

### Summary

You have successfully set up:

- A **REST API** for game logic communication  
- A **WebSocket API** for real-time leaderboard  
- Multiple **Lambda functions** (ZIP + Container) for all backend processing  

These services form the **core backend infrastructure** for the game, enabling secure API access, real-time updates, and AI-powered avatar processing.

