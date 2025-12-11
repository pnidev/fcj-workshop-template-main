---
title : "Prerequiste"
date :  "`r Sys.Date()`" 
weight : 2 
chapter : false
pre : " <b> 5.2. </b> "
---

#### IAM Permissions

Add the following IAM policy to your user account to deploy and manage the game backend project:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "GameProjectPermissions",
      "Effect": "Allow",
      "Action": [
        "lambda:*",
        "apigateway:*",
        "cognito-idp:*",
        "dynamodb:*",
        "s3:*",
        "cloudwatch:*",
        "iam:PassRole",
        "codebuild:*",
        "codepipeline:*",
        "logs:*",
        "sns:*"
      ],
      "Resource": "*"
    }
  ]
}