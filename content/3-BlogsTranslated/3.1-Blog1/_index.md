---
title: "Blog 1"
date: "`r Sys.Date()`"
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---
{{% notice warning %}}
⚠️ **Note:** The information below is for reference purposes only. Please do **not copy verbatim** for your report, including this warning.
{{% /notice %}}

# Querying Amazon Aurora PostgreSQL using Amazon Bedrock Knowledge Bases with structured data
Amazon Bedrock Knowledge Bases provides a fully managed Retrieval Augmented Generation (RAG) capability that allows Large Language Models (LLMs) to connect with internal data sources. This feature enhances the output of Foundation Models (FMs) by augmenting them with context from private datasets, making responses more accurate and relevant.

At AWS re:Invent 2024, AWS announced that Amazon Bedrock Knowledge Bases now supports natural language querying for retrieving structured data from Amazon Redshift and Amazon SageMaker Lakehouse.
This capability enables a complete workflow for building Generative AI applications capable of accessing and integrating data from both structured and unstructured sources. Using Natural Language Processing (NLP), Amazon Bedrock Knowledge Bases can convert natural language queries into SQL statements, enabling users to retrieve data from supported sources without knowing database structure or SQL syntax.

This article demonstrates how to make data stored in Amazon Aurora PostgreSQL-Compatible Edition queryable using natural language through Amazon Bedrock Knowledge Bases, while still maintaining data freshness.



## Structured data retrieval in Amazon Bedrock Knowledge Bases and Amazon Redshift Zero-ETL
Structured data retrieval in Amazon Bedrock Knowledge Bases allows natural language interaction with your database by converting user queries into SQL statements. When you connect a supported data source such as Amazon Redshift, Amazon Bedrock Knowledge Bases analyzes the database schema, table relationships, query engine, and historical queries to understand data context and structure. This knowledge enables the service to generate accurate SQL queries based on natural language questions.

At the time of writing, Amazon Bedrock Knowledge Bases supports structured data retrieval directly from Amazon Redshift and SageMaker Lakehouse. Although direct support for Aurora PostgreSQL-Compatible is not currently available, you can use zero-ETL integration between Aurora PostgreSQL-Compatible and Amazon Redshift to make your data accessible to Amazon Bedrock Knowledge Bases structured data retrieval. Zero-ETL integration automatically replicates Aurora PostgreSQL tables into Amazon Redshift near real-time, eliminating the need for complex ETL pipelines or data movement processes.

This architectural pattern is especially valuable for organizations that want to enable natural language querying over operational application data stored in Aurora PostgreSQL database tables. By combining zero-ETL integration with Amazon Bedrock Knowledge Bases, you can build powerful applications such as AI assistants powered by LLMs that deliver natural language responses based on operational data.

## Solution overview
The following diagram illustrates the architecture we will deploy to connect Aurora PostgreSQL-Compatible with Amazon Bedrock Knowledge Bases using zero-ETL.

![blog1_1](/images/blog1_1.png)

Workflow includes the following steps:
Data is stored in Aurora PostgreSQL-Compatible inside a private subnet. A bastion host is used to securely connect to the database from a public subnet.


Using zero-ETL integration, this data is streamed into Amazon Redshift, also within a private subnet.


Amazon Bedrock Knowledge Bases uses Amazon Redshift as the structured data source.


Users can interact with Amazon Bedrock Knowledge Bases through AWS Management Console or AWS SDK clients, submitting natural language queries. These queries are processed by Amazon Bedrock Knowledge Bases to retrieve information stored in Redshift (originating from Aurora).


## Prerequisites
 Ensure you are logged in with an IAM role that can:
Create an Aurora database and run DDL (CREATE, ALTER, DROP, RENAME) and DML (SELECT, INSERT, UPDATE, DELETE) statements


Create a Redshift database


Configure zero-ETL integration


Create an Amazon Bedrock knowledge base


## Set up Aurora PostgreSQL database
 In this section, we walk through creating and configuring an Aurora PostgreSQL database with a sample schema for demonstration. We will create three related tables: products, customers, and orders.


## Provision the database
In this section, we walk through creating and configuring an Aurora PostgreSQL database with a sample schema for demonstration. We will create three related tables: products, customers, and orders.
Provision the database
 Start by provisioning a database environment. Create a new Aurora PostgreSQL database cluster and launch an EC2 instance to serve as a management entry point. The EC2 instance will help us create tables and maintain data throughout this guide.
The following screenshot shows the details of the database cluster and EC2 instance.
![blog1_1](/images/blog1_2.png)

To follow the instructions for setup, reference Creating and connecting to an Aurora PostgreSQL DB cluster.
Create the database schema
 Once connected to the database over SSH via your EC2 instance (as shown in Creating and connecting to an Aurora PostgreSQL DB cluster), it's time to define the data structure. Use the following DDL statements to create three tables:
-- Create Product table
CREATE TABLE product (
    product_id SERIAL PRIMARY KEY,
  product_name VARCHAR(100) NOT NULL,
    price DECIMAL(10, 2) NOT NULL
);

-- Create Customer table
CREATE TABLE customer (
    customer_id SERIAL PRIMARY KEY,
    customer_name VARCHAR(100) NOT NULL,
    pincode VARCHAR(10) NOT NULL
);

-- Create Orders table
CREATE TABLE orders (
    order_id SERIAL PRIMARY KEY,
    product_id INTEGER NOT NULL,
    customer_id INTEGER NOT NULL,
    FOREIGN KEY (product_id) REFERENCES product(product_id),
    FOREIGN KEY (customer_id) REFERENCES customer(customer_id)
);


## Populate data into the tables
After tables are created, insert sample data. Ensure referential integrity is preserved when inserting into orders by confirming product_id exists in product and customer_id exists in customer.

Example inserts:

INSERT INTO product (product_id, product_name, price) VALUES (1, 'Smartphone X', 699.99);
INSERT INTO product (product_id, product_name, price) VALUES (2, 'Laptop Pro', 1299.99);
INSERT INTO product (product_id, product_name, price) VALUES (3, 'Wireless Earbuds', 129.99);
INSERT INTO customer (customer_id, customer_name, pincode) VALUES (1, 'John Doe', '12345');
INSERT INTO customer (customer_id, customer_name, pincode) VALUES (2, 'Jane Smith', '23456');
INSERT INTO customer (customer_id, customer_name, pincode) VALUES (3, 'Robert Johnson', '34567');
INSERT INTO orders (order_id, product_id, customer_id) VALUES (1, 1, 1);
INSERT INTO orders (order_id, product_id, customer_id) VALUES (2, 1, 2);
INSERT INTO orders (order_id, product_id, customer_id) VALUES (3, 2, 3);
INSERT INTO orders (order_id, product_id, customer_id) VALUES (4, 2, 1);
INSERT INTO orders (order_id, product_id, customer_id) VALUES (5, 3, 2);
INSERT INTO orders (order_id, product_id, customer_id) VALUES (6, 3, 3);

Be sure to maintain referential integrity when inserting orders to avoid foreign key violations.

You may apply similar data samples to build schema and populate other tables.


## Staging ER7 microservice

- Lambda “trigger” registered with pub/sub hub, filtering messages by attribute  
- Step Functions Express Workflow converts ER7 → JSON  
- Two Lambdas:
  1. Format cleanup for ER7 (newline, carriage return)
  2. Parsing logic  
- Results or errors are pushed back into pub/sub hub

---

## Configure Redshift cluster and zero-ETL
 After the Aurora database is prepared, proceed to configure zero-ETL integration with Amazon Redshift. This integration automatically synchronizes data from Aurora PostgreSQL-Compatible into Redshift.


### Configure Amazon Redshift
First, create a Redshift Serverless workgroup and namespace. Refer to Creating a data warehouse with Amazon Redshift Serverless.
### Create zero-ETL integration
 Zero-ETL integration involves two main steps:

1. Create a zero-ETL integration from Aurora PostgreSQL to Redshift Serverless.
2. After enabling integration on Aurora, create a corresponding database mapping on Redshift to ensure synchronized replication.

The screenshot below illustrates our zero-ETL integration setup.
![blog1_1](/images/blog1_3.png)
### Verification
 After configuration, verify successful integration.

You may confirm in the Redshift console by checking zero-ETL integration status. You should see Active along with source and destination as shown below.

![blog1_1](/images/blog1_4.png)
Additionally, use Redshift Query Editor v2 to verify replicated data. A simple query like SELECT * FROM customer; should return the synchronized dataset from Aurora, as shown below:
![blog1_1](/images/blog1_5.png)

### Configure Amazon Bedrock knowledge base with structured data
The final step is to create an Amazon Bedrock knowledge base that supports structured querying.

#### Create Amazon Bedrock knowledge base

Create a new Amazon Bedrock knowledge base with structured data selected. Refer to Build a knowledge base by connecting to a structured data store. After that, synchronize the query engine to grant data access.

#### Grant data permissions
 Before synchronization succeeds, ensure appropriate permissions have been granted to the Amazon Bedrock Knowledge Base IAM role. This includes issuing GRANT SELECT commands for each table within Redshift.

Run the following in Redshift Query Editor v2 per table:
 GRANT SELECT ON <table_name> TO "IAMR:<KB Role name>";
 Example:
 GRANT SELECT ON customer TO "IAMR:AmazonBedrockExecutionRoleForKnowledgeBase_ej0f0";

For production environments, end-user identity must be federated for role-based data access. Reference AWS documentation for structured database access using identity federation. For web client identity federation, Amazon Cognito or SAML with AWS STS may be required based on your architecture.

### Confirm setup
Once completed, your knowledge base should show:
Status = Available


Query engine successfully synchronized with Amazon Redshift


Database synchronization = COMPLETE



You may now query data using natural language.

### Example natural language queries
Once Amazon Bedrock knowledge base is active, you may test structured data retrieval through natural language prompts. The service generates SQL automatically, runs the query, and uses LLMs to return readable responses.

You may test using:

● Amazon Bedrock console → Knowledge Bases → Test tab  


● AWS SDK or API → Execute RetrieveAndGenerate programmatically  


Console offers two modes of interaction:

● Generate SQL queries – Shows raw SQL created from natural language input.
![blog1_1](/images/blog1_6.png)


● Retrieval and response generation – Generates natural language output using an LLM.
Table below shows example queries, SQL generated, and final responses.
![blog1_1](/images/blog1_7.png)

| **Natural Language Query** | **Generate SQL Output** | **Response Output** | **Model** |
|---|---|---|---|
| How many customers do we have? | SELECT COUNT(DISTINCT customer_id) AS num_customers FROM public.customer; | We currently have 11 unique customers. | Amazon Nova Lite |
| Which customers have purchased the most products? | SELECT c."customer_name", COUNT(o."product_id") AS "total_products_purchased" FROM public.customer c INNER JOIN public.orders o ON c."customer_id" = o."customer_id" GROUP BY c."customer_name" ORDER BY "total_products_purchased" DESC; | Based on the data, the customers who have purchased the most products are Charlie Davis, Alice Brown, and John Doe, each having purchased 14 products. Following closely are Jane Smith, Grace Lee, and Bob Johnson, who have each purchased 13 products. Henry Taylor, Frank Miller, and Eva Wilson have each purchased 12 products, while Ivy Anderson has purchased 11 products. | Amazon Nova Lite |
| Which customers have purchased more than one of the most expensive product? | SELECT c.customer_name FROM public.customer c INNER JOIN public.orders o ON c.customer_id = o.customer_id INNER JOIN public.product p ON o.product_id = p.product_id WHERE p.price = (SELECT MAX(price) FROM public.product) GROUP BY c.customer_name HAVING COUNT(DISTINCT o.order_id); | The customers who have purchased more than one number of the most expensive product are Grace Lee, Jane Smith, Alice Brown, and Eva Wilson. | Amazon Nova Micro |

● Retrieval and response generation – Generates natural language output using an LLM.
![blog1_1](/images/blog1_7.png)
### Cleanup

When finished, delete the resources created to avoid ongoing charges.

### Conclusion
 This post demonstrated how to enable natural language querying on Aurora PostgreSQL data using Amazon Bedrock Knowledge Bases via zero-ETL integration with Amazon Redshift. We covered database setup, zero-ETL configuration, and knowledge base integration to retrieve data seamlessly. While this approach provides highly effective natural language access to structured data, consider Redshift storage cost when deploying in production.

Try this solution and share your feedback in the comments section.

### About the authors

![blog1_1](/images/tacgia1.png)
Girish B is a Senior Solutions Architect at AWS India Pvt Ltd, based in Bengaluru. Girish works with various ISV customers to design and architect innovative solutions on AWS.

![blog1_1](/images/tacgia2.png)
Dani Mitchell is a Generative AI Specialist Solutions Architect at AWS. She focuses on helping organizations worldwide accelerate their Generative AI journey with Amazon Bedrock.
