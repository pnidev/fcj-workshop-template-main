---
title: "Translated Blogs"
date: "`r Sys.Date()`"
weight: 3
chapter: false
pre: " <b> 3. </b> "
---

{{% notice warning %}}
⚠️ **Note:** The information below is for reference purposes only. Please **do not copy verbatim** for your own report, including this warning.
{{% /notice %}}

This section will list and introduce the blogs you have translated. For example:

###  [Blog 1 - Getting started with healthcare data lakes: Using microservices](3.1-Blog1/)
This blog explains how to make data stored in Aurora PostgreSQL “queryable in natural language” using Amazon Bedrock Knowledge Bases. The authors use zero-ETL to automatically replicate data from Aurora to Amazon Redshift, then Bedrock Knowledge Bases converts English questions into SQL, runs them on Redshift, and returns human-readable answers. It walks through creating a sample schema (product, customer, orders), configuring zero-ETL, creating a knowledge base, and testing questions like “How many customers do we have?”.

###  [Blog 2 - ...](3.2-Blog2/)
This blog shows how to use Powertools for AWS Lambda (TypeScript) with the new Parser utility to validate event payloads (SQS, EventBridge, etc.) in a clean and maintainable way. The author uses the Zod library to define schemas (for example, an orderSchema), and the parser then parses and validates the event, can automatically extract the detail/body via “envelopes”, and supports safeParse so you can handle errors, logging, and metrics yourself. The post also demonstrates how to extend built-in schemas and add complex business rules using .refine().

###  [Blog 3 - ...](3.3-Blog3/)
This blog describes a multi-tenant, multi-account architecture for Generative AI use cases using Amazon Bedrock. The authors design a hub account as a shared entry point (ALB, Cognito, routing, authorization) and multiple spoke accounts dedicated to each tenant, all connected via AWS Transit Gateway. The hub receives requests, authenticates, routes them to a tenant-specific Lambda, which assumes a cross-account role and calls Bedrock in the tenant’s VPC, allowing centralized control (auth, routing) while preserving isolation, quotas, costs, and policies for each tenant.