---
title: "Blog 2"
date: "`r Sys.Date()`"
weight: 1
chapter: false
pre: " <b> 3.2. </b> "
---

{{% notice warning %}}
⚠️ **Note:** The information below is for reference purposes only. Please **do not copy it verbatim** into your report, including this warning.
{{% /notice %}}

# Scaling Generative AI use cases – Part 1: Hub-and-spoke multi-tenant architecture using AWS Transit Gateway
Generative AI continues to shape how enterprises approach innovation and problem solving. Customers are moving from the experimentation phase to scaling generative AI use cases across their organizations, with more and more businesses fully integrating these technologies into core processes. This evolution spans multiple lines of business (LOBs), teams, and Software-as-a-Service (SaaS) providers. Although many AWS customers typically start with a single AWS account to run generative AI proofs of concept, increased adoption and a shift to production environments have introduced new challenges.

These challenges include effectively managing and scaling deployments as well as abstracting and reusing cross-cutting concerns such as multi-tenancy, isolation, authentication, authorization, secure networking, rate limiting, and caching. To address these challenges efficiently, a multi-account architecture proves beneficial, particularly for SaaS providers serving many enterprise customers, large corporations with distinct business units, and organizations with strict compliance requirements. This multi-account approach helps you maintain a well-architected system by providing better organization, stronger security, and greater scalability in your AWS environment. It also enables you to manage these cross-cutting concerns more effectively as you scale your generative AI deployments.

In this two-part series, we discuss a hub-and-spoke architectural model for building a multi-tenant, multi-account architecture. This pattern supports abstraction of shared services across use cases and teams, helping you build secure, scalable, and reliable generative AI systems. In Part 1, we introduce a centralized hub for generative AI service abstractions and tenant-specific spokes, using AWS Transit Gateway to connect accounts. The hub account acts as the entry point for end-user requests, centralizing common capabilities such as authentication, authorization, model access, and routing decisions. This approach reduces the need to implement these capabilities separately in each spoke account. Where possible, we use VPC endpoints to access AWS services.

In Part 2, we discuss a variation of this architecture that uses AWS PrivateLink to securely share a centralized endpoint in the hub account with teams within the organization or with external partners.

The focus in both posts is on centralizing authentication, authorization, model access, and multi-account secure networking to onboard and scale generative AI use cases with Amazon Bedrock. We do not cover other system capabilities such as prompt catalog, prompt caching, versioning, model registry, and cost. However, those capabilities can be layered on as extensions to this architecture.

## Solution overview
Our solution implements a hub-and-spoke pattern that provides a secure, scalable system for managing generative AI deployments across multiple accounts. At its core, the architecture consists of a centralized hub account that serves as the entry point for requests, complemented by spoke accounts that host tenant-specific resources. The following diagram illustrates this architecture:
![blog1_1](/images/blog2_1.png)

The hub account acts as the central account, providing shared services for multiple tenants and serving as the entry point for end-user requests. It centralizes common capabilities such as authentication, authorization, and routing decisions, eliminating the need to implement these functions separately for each tenant. The hub account is operated and maintained by the core engineering team.

The hub infrastructure includes public and private VPCs, an internet-facing Application Load Balancer (ALB), Amazon Cognito for authentication, and the necessary VPC endpoints for AWS services.

The spoke accounts host tenant-specific resources, such as AWS Identity and Access Management (IAM) role permissions and Amazon Bedrock resources. Spoke accounts may be managed by the core engineering team or by the tenants themselves, depending on organizational requirements.

Each spoke account maintains its own private VPC, VPC interface endpoints for Amazon Bedrock, tenant-specific IAM roles and permissions, and other account-level controls. These components are connected via a Transit Gateway, which provides secure cross-account networking and manages traffic between the hub and spoke VPCs. The request flow through the system (as shown in the earlier architecture diagram) includes the following steps:
A user (representing Tenant 1, 2, or N) accesses the client application.

The client application in the public subnet of the hub account authenticates the user and receives an ID/JWT token. In this example, we use an Amazon Cognito user pool as the identity provider (IdP).

The client application uses custom attributes in the JWT token to determine the appropriate route on the ALB. The ALB, based on the context path, routes the request to the tenant’s AWS Lambda function target group.

The tenant-specific Lambda function in the private subnet of the hub account is invoked.

This function assumes a cross-account role in the tenant’s account. The function calls Amazon Bedrock in the spoke account by referencing the regional DNS name of the Amazon Bedrock VPCE. The model is invoked and the result is returned to the user.

This architecture ensures that all requests pass through a central entry point while maintaining tenant isolation. By calling Amazon Bedrock in the spoke account, each request inherits that account’s limits, access controls, cost assignments, service control policies (SCPs), and other account-level controls.

Sample source code for this solution is split into two parts:

The first part demonstrates the solution for a single hub-and-spoke account.

The second part extends the solution by deploying an additional spoke account.

Detailed, step-by-step instructions are provided in the repository README. In the following sections, we provide an outline of the deployment steps.

## Prerequisites
We assume that you are familiar with the basics of AWS networking, including Amazon Virtual Private Cloud (Amazon VPC) and VPC constructs such as route tables and VPC interconnectivity options. We also assume that you understand multi-tenant architectures and the core principles of serving multiple tenants on shared infrastructure while maintaining isolation.

To deploy this solution, you need the following prerequisites:
Hub and spoke accounts (required):

Two AWS accounts: one hub account and one spoke account

Access to the amazon.titan-text-lite-v1 model in the spoke account

Additional spoke account (optional):

A third AWS account (a spoke account for a second tenant)

Access to the anthropic.claude-3-haiku-20240307-v1:0 model in the second spoke account

## Design considerations
Deploying this architecture involves several important design choices that affect how the solution operates, scales, and can be maintained. In this section, we examine these design considerations across the different components, explaining the rationale behind each choice and possible alternatives where appropriate.

## Lambda functions
In our design, the ALB target group is configured with Lambda functions running in the hub account instead of the spoke accounts. This approach enables centralized management of business logic, as well as centralized logging and monitoring. As the architecture evolves to include cross-cutting capabilities such as prompt caching, semantic routing, or the use of large language model (LLM) proxies (middleware services that provide unified access to multiple models while handling rate limiting and request routing, as discussed in Part 2), implementing these capabilities in the hub account ensures consistency across tenants.

We chose Lambda functions to implement token validation and routing logic, but you can also use other compute options such as Amazon Elastic Container Service (Amazon ECS) or Amazon Elastic Kubernetes Service (Amazon EKS), depending on your organization’s preferences.

We use a 1-to-1 mapping between Lambda functions and tenants. Although the current logic in each function is similar, having a dedicated function for each tenant can help reduce noisy neighbor issues and support tenant-tier-specific configurations such as memory and concurrency.

## VPC endpoints
In this solution, we use dedicated Amazon Bedrock runtime VPC endpoints in the spoke accounts. Using dedicated VPC endpoints for each spoke account is suitable for organizations where operators of the spoke accounts are responsible for managing tenant features such as model access, knowledge bases, and guardrails.

Depending on your organization’s policies, you may implement a different variant of this architecture by using centralized Amazon Bedrock runtime VPC endpoints in the hub account (as described in Part 2). Centralized VPC endpoints are suitable for organizations where a central engineering team manages features for tenants.

Other factors such as cost, access control, and endpoint quotas must be considered when choosing between a centralized or dedicated approach for the placement of Amazon Bedrock VPC endpoints. With a centralized approach, VPC endpoint policies may hit the 20,480-character limit as the number of tenants increases. There are hourly fees for VPC endpoints and Transit Gateway attachments that are provisioned regardless of usage. If VPC endpoints are provisioned in spoke accounts, each tenant incurs additional hourly fees.

## Client application
For demonstration purposes, the client application in this solution is deployed in the public subnet of the hub VPC. The application can be deployed in an account outside both the hub and spoke VPCs, or deployed at the edge as a single-page application (SPA) using Amazon CloudFront and Amazon Simple Storage Service (Amazon S3).

## Tenancy
Enterprises use different tenancy models when scaling generative AI, each with its own advantages and disadvantages. Our solution implements a silo model, assigning each tenant to a dedicated spoke account. For smaller organizations with fewer tenants and less stringent isolation requirements, an alternative approach is the pooled model (multiple tenants in a single spoke account), which may be more appropriate—unless they plan to scale significantly in the future or have specific compliance requirements. For more information on multi-tenancy design, see *Let’s Architect! Designing architectures for multi-tenancy.* Cell-based architectures for multi-tenant applications can provide benefits such as fault isolation and scaling. See *Reducing the Scope of Impact with Cell-Based Architecture* for more details.

## Frontend gateway

In this solution, we choose ALB as the entry point for requests. ALB offers several advantages for our generative AI use case:

Long-running connections – ALB supports connections up to 4,000 seconds, which is beneficial for LLM responses that may take more than 30 seconds to complete.

Scalability – ALB can handle a large volume of concurrent connections, making it suitable for enterprise-scale deployments.

Integration with AWS WAF – ALB integrates seamlessly with AWS WAF, providing enhanced security and protection against common web exploits.

Amazon API Gateway is an alternative when you need API versioning, usage plans, or granular API management capabilities, and when message sizes and response times fit within its quotas. AWS AppSync is another option suitable when you want to expose LLMs through a GraphQL interface.

Choose the gateway that best fits your customers:

ALB efficiently handles high-volume, long-running connections.

API Gateway provides comprehensive REST API management.

AWS AppSync delivers real-time GraphQL capabilities.

Evaluate each option based on response time requirements, API needs, scale demands, and the specific use case of your application.

Although this post demonstrates HTTP connectivity for simplicity, this is not recommended for production.  
Production deployments must always use HTTPS with proper SSL/TLS certificates to maintain secure communication.

## IP addressing
The AWS CloudFormation template for deploying solution resources uses example CIDRs. When deploying this architecture in a second spoke account, use unique IP address ranges that do not overlap with existing environments. Transit Gateway operates at Layer 3 and requires distinct IP spaces to route traffic between VPCs.

## Deploy a hub and spoke account
In this section, we set up a local AWS Command Line Interface (AWS CLI) environment to deploy this solution in two AWS accounts. Detailed instructions are provided in the repository README.
Deploy a CloudFormation stack in the hub account and another stack in the spoke account.

Configure connectivity between the hub and spoke VPCs using Transit Gateway attachments.

Create an Amazon Cognito user with the value tenant1 for a custom user attribute named tenant_id.

Create an item in the Amazon DynamoDB table to map the tenant ID to tenant-specific model access and routing information, in our example tenant1.
![blog1_1](/images/blog2_2.png)
![blog1_1](/images/blog2_3.png)

## Validate connectivity
In this section, we validate connectivity from a test application in the hub account to an Amazon Bedrock model in the spoke account. We do this by sending a curl request from an EC2 instance (representing our client application) to the ALB. Both the EC2 instance and the ALB reside in the public subnet of the hub account. The request and response are then routed through Transit Gateway attachments between the hub and spoke VPCs. The following screenshot shows the execution of a utility script on your local workstation that authenticates a user and exports the necessary variables. These variables will be used to construct the curl request on the EC2 instance.

![blog1_1](/images/blog2_4.png)

The next screenshot shows the curl request being executed from the EC2 instance to the ALB. The response confirms that the request was processed successfully and served by the amazon.titan-text-lite-v1 model, which is the model mapped to this user (tenant1). This model is hosted in the spoke account.
![blog1_1](/images/blog2_5.png)

## Deploy a second spoke account
In this section, we extend the deployment to include a second spoke account for an additional tenant.  
We validate multi-tenant connectivity by sending another curl request from the same EC2 instance to the ALB in the hub account.  
Detailed instructions are provided in the repository README.  
The following screenshot shows the response to this request, demonstrating that the system correctly identifies and routes requests based on tenant information.  
In this case, the user’s tenant_id attribute value is tenant2, and the request is successfully routed to the anthropic.claude-3-haiku-20240307-v1:0 model, which is mapped to tenant2 in the second spoke account.

## Clean up
To clean up your resources, complete the following steps:
If you created optional resources for the second spoke account, delete them:
Change the directory to  
 genai-secure-patterns/hub-spoke-transit-gateway/scripts/optional

Run the cleanup script  
 ./cleanupOptionalStack.sh

Clean up the main stack:
Change the directory to  
 genai-secure-patterns/hub-spoke-transit-gateway/scripts/

Run the cleanup script  
 ./cleanup.sh

## Conclusion
As organizations increasingly adopt and scale generative AI use cases across multiple teams and lines of business (LOBs), the need for secure, scalable, and reliable multi-tenant architectures continues to grow.  
This two-part series addresses that need by providing guidance on how to implement the hub-and-spoke architecture pattern.  
By adopting such well-architected practices early, you can build scalable and robust solutions that unlock the full potential of generative AI in your organization.

In this post, we showed how to set up a centralized hub account hosting shared services such as authentication, authorization, and networking using Transit Gateway.  
We also demonstrated how to configure spoke accounts to host tenant-specific resources such as Amazon Bedrock.  
Try the provided code samples to see how this architecture works in practice.  
Part 2 will explore an alternative implementation using PrivateLink to interconnect VPCs in the hub and spoke accounts.

## About the Authors

![blog1_1](/images/blog2_6.png)
Nikhil Penmetsa is a Senior Solutions Architect at AWS. He helps organizations understand best practices related to advanced cloud-based solutions. He is passionate about diving deep with customers to build solutions that are cost-effective, secure, and performant. When he is not in the office, you can often find him putting in miles on his road bike or hitting the open road on his motorcycle.

![blog1_1](/images/blog2_7.jpg)
Ram Vittal is a Principal ML Solutions Architect at AWS. He has over three decades of experience architecting and building distributed, hybrid, and cloud applications. He is passionate about building secure and scalable AI/ML and big data solutions to help enterprise customers in their cloud adoption and optimization journeys to improve their business outcomes. In his spare time, he rides his motorcycle and walks with his 3-year-old Sheepadoodle!
```
