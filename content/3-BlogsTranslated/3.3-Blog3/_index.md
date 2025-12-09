---
title: "Blog 3"
date: "`r Sys.Date()`"
weight: 1
chapter: false
pre: " <b> 3.3. </b> "
---

{{% notice warning %}}
⚠️ **Note:** The information below is for reference purposes only. Please **do not copy it verbatim** into your report, including this warning.
{{% /notice %}}

# Validating event payloads with Powertools for AWS Lambda (TypeScript)
In this post, learn how Powertools for AWS Lambda (TypeScript) with the new Parser utility can help you validate payloads easily and make your Lambda function more resilient.  
Validating input payloads is an important aspect of building secure and reliable applications. It ensures that data received by an application can smoothly handle unexpected or malicious inputs and prevent harmful downstream processing. When writing AWS Lambda functions, developers need to validate and verify the payload, while ensuring that specific fields and values are correct and safe to process.

Powertools for AWS Lambda is a developer toolkit available for Python, NodeJS/TypeScript, Java, and .NET. It helps you implement serverless best practices and increase developer velocity. Powertools for AWS Lambda (TypeScript) now introduces a new Parser utility to make it easier for developers to implement validation in their Lambda functions.

## Why payload validation matters
Validating payloads can help your Lambda functions become more resilient. Payloads often combine both technical and business information, which can make them harder to validate. This requires you to write validation logic inside your Lambda function code. This can range from a few if-statements to check payload values to a chain of complex validation steps based on custom business logic. You may need to separate validation of technical information in the payload, such as AWS Region, accountId, event source, from business information in the event, such as productId and payment details.

Understanding the structure and values of the event object, as well as how to extract the relevant information, can be challenging. For example, an Amazon SQS event has a body field with a string value that can be a JSON document. Amazon EventBridge has an object in the detail field that you can read directly without further transformation. You might need to decompress, decode, transform, and validate a payload inside a specific field. Understanding these multiple transformation layers can become complex, especially if your event object is the result of multiple service invocations.

## Using Powertools for AWS Lambda (TypeScript) Parser Utility
Powertools for AWS Lambda (TypeScript) is a modular library. You can selectively install features such as Logger, Tracer, Metrics, Batch Processing, Idempotency, and more. You can use Powertools for AWS Lambda in both TypeScript and JavaScript code bases. This new Parser utility simplifies validation and uses the popular validation library Zod.

You can use the parser as a method decorator, with middyjs middleware, or manually in all NodeJS runtimes provided by Lambda.

To use the utility, install the Powertools parser utility and Zod (<v3.x) using NPM or any package manager of your choice:
npm install @aws-lambda-powertools/parser zod@~3
You can define your schema using Zod. Below is an example of a simple order schema to validate events:
import { z } from 'zod';
const orderSchema = z.object({
    id: z.number().positive(),
	description: z.string(),
	items: z.array(
	    z.object({
		    id: z.number().positive(),
			quantity: z.number(),
			description: z.string(),
			})
		),
	});
export { orderSchema };
The following schema defines id, description, and a list of items. You can specify value types ranging from simple numeric values, narrow them down to positive or literal values, or use more complex types such as unions, arrays, or even other schemas. Zod provides a rich set of value types that you can use.

Add the parser decorator to your handler function, set the schema parameter, and use this schema to parse the event object.
import type {Context} from 'aws-lambda';
import type {LambdaInterface} from '@aws-lambda-powertools/commons/types';
import {parser} from '@aws-lambda-powertools/parser';
import {z} from 'zod';
import {Logger} from '@aws-lambda-powertools/logger';

const logger = new Logger();

const orderSchema = z.object({
    id: z.number().positive(),  
	description: z.string(),  
	items: z.array(
	    z.object({
		    id: z.number().positive(),
			quantity: z.number(),
			description: z.string(),
		})
	),
});

type Order = z.infer<typeof orderSchema>;

class Lambda implements LambdaInterface {
    @parser({schema: orderSchema})  
	public async handler(event: Order, _context: Context): Promise<void> {
	    // event is now typed as Order    
		for (const item of event.items) {      
		    logger.info('Processing item', {item});
			// process order item from the event
		}
	}
}
	
const myFunction = new Lambda();

export const handler = myFunction.handler.bind(myFunction);
Note that z.infer helps extract the Order type from the schema, which improves the development experience with autocomplete when using TypeScript. Zod parses the entire object, including nested fields, and reports all combined errors instead of only returning the first error.

## Using built-in schemas for AWS services
A more common scenario is needing to validate events from AWS Services that trigger Lambda functions, including Amazon SQS, Amazon EventBridge, and many others. To make this easier, Powertools includes pre-built schemas for AWS events that you can use.

To parse an incoming Amazon EventBridge event, configure the built-in schema in your parser configuration:

import {LambdaInterface} from '@aws-lambda-powertools/commons/types';
import {Context} from 'aws-lambda';
import {parser} from '@aws-lambda-powertools/parser';
import {EventBridgeSchema} from '@aws-lambda-powertools/parser/schemas';
import type {EventBridgeEvent} from '@aws-lambda-powertools/parser/types';

class Lambda implements LambdaInterface {  
    @parser({schema: EventBridgeSchema})  
	public async handler(event: EventBridgeEvent, _context: Context): Promise<void> {    
	    // event is parsed but the detail field is not specified  
	}
}

const myFunction = new Lambda();

export const handler = myFunction.handler.bind(myFunction);

The event object is parsed and validated at runtime, and the EventBridgeEvent TypeScript type helps you understand the structure and access fields during development. In this example, you only parse the EventBridge event object, so the detail field can be an arbitrary object.  
You can also extend the built-in EventBridge schema and override the detail field with your custom orderSchema:

import {LambdaInterface} from '@aws-lambda-powertools/commons/types';
import {Context} from 'aws-lambda';
import {parser} from '@aws-lambda-powertools/parser';
import {EventBridgeSchema} from '@aws-lambda-powertools/parser/schemas';
import {z} from 'zod';

const orderSchema = z.object({  
    id: z.number().positive(),  
	description: z.string(),  
	items: z.array(
	    z.object({
		    id: z.number().positive(),      
			quantity: z.number(),      
			description: z.string(),
		}), 
	),
});

const eventBridgeOrderSchema = EventBridgeSchema.extend({  detail: orderSchema,});

type EventBridgeOrder = z.infer<typeof eventBridgeOrderSchema>;

class Lambda implements LambdaInterface {  
    @parser({schema: eventBridgeOrderSchema})  public async handler(event: EventBridgeOrder, _context: Context): Promise<void> {
	    // event.detail is now parsed as orderSchema  
	}
}

const myFunction = new Lambda();

export const handler = myFunction.handler.bind(myFunction);

The parser will validate the entire structure of the EventBridge event, including the custom business object.  
Use .extend or other Zod schema functions to modify any field of the built-in schema and customize payload validation.

## Using envelopes with custom schemas
In some cases, you only need the custom part of the payload, for example the detail field of an EventBridge event or the body of SQS records. This requires you to parse the event schema manually, extract the field you need, and then parse again using a custom schema. This is complex because you must know exactly which payload field to process and how to transform and parse it.

The Powertools Parser utility addresses this problem using Envelopes. Envelopes are schema objects with built-in logic to extract custom payloads.

The following is an example of EventBridgeEnvelope and how it works:
import {LambdaInterface} from '@aws-lambda-powertools/commons/types';
import {Context} from 'aws-lambda';
import {parser} from '@aws-lambda-powertools/parser';
import {EventBridgeEnvelope} from '@aws-lambda-powertools/parser/envelopes';
import {z} from 'zod';

const orderSchema = z.object({  
    id: z.number().positive(), 
	description: z.string(),  
	items: z.array(    
	    z.object({      
		    id: z.number().positive(),      
			quantity: z.number(),      
			description: z.string(),
		}), 
	),
});

type Order = z.infer<typeof orderSchema>;

class Lambda implements LambdaInterface {  
    @parser({schema: orderSchema, envelope: EventBridgeEnvelope})  public async handler(event: Order, _context: Context): Promise<void> {
        // event is now typed as Order inferred from the orderSchema  
    }
}

const myFunction = new Lambda();

export const handler = myFunction.handler.bind(myFunction);

By setting both the schema and the envelope, the parser utility knows how to combine both parameters, extract, and validate the custom payload from the event.  
Powertools Parser transforms the event object according to the schema definition, allowing you to focus on the business-critical portion inside your handler function.

## Safe parsing
If the object does not match the provided Zod schema, by default the parser throws a ParserError. If you need to control validation errors and want to implement custom error handling, use the safeParse option.

The following is an example of how to capture failed validations as a metric in your handler function:
import {Logger} from "@aws-lambda-powertools/logger";
import {LambdaInterface} from "@aws-lambda-powertools/commons/types";
import {parser} from "@aws-lambda-powertools/parser";
import {orderSchema} from "../app/schema";import {z} from "zod";
import {EventBridgeEnvelope} from "@aws-lambda-powertools/parser/envelopes";
import {Metrics, MetricUnit} from "@aws-lambda-powertools/metrics";
import {ParsedResult, EventBridgeEvent} from "@aws-lambda-powertools/parser/types";

const logger = new Logger();

const metrics = new Metrics();

type Order = z.infer<typeof orderSchema>;

class Lambda implements LambdaInterface {  

    @metrics.logMetrics() 
	@parser({schema: orderSchema, envelope: EventBridgeEnvelope, safeParse: true})  
	public async handler(event: ParsedResult<EventBridgeEvent, Order>, _context: unknown): Promise<void> {
	    if (!event.success) {      
		    // failed validation      
			metrics.addMetric('InvalidPayload', MetricUnit.Count, 1);      
			logger.error('Invalid payload', event.originalEvent);    
		} else {      
		    // successful validation      
			for (const item of event.data.items) {        
			    logger.info('Processing item', item);        
				// event.data is typed as Order      
			}   
		}  
	}
}

const myFunction = new Lambda();

export const handler = myFunction.handler.bind(myFunction);

When safeParse = true, the parser does not throw an error. Instead, it returns a modified event object with a success flag and either error or data fields depending on the validation result.  
You can implement custom error handling, for example incrementing an InvalidPayload metric and accessing originalEvent to log the error.  
For successful validations, you can access the data field and process the payload.  
Note that the event object type is now ParsedResult, with EventBridgeEvent as the input and Order as the output type.

## Custom validations
Sometimes you may need more complex business rules for validation.  
Because Parser built-in schemas are Zod objects, you can customize validation by applying .extend, .refine, .transform, and other Zod operators.

Below is an example of complex rules for orderSchema:
import {z} from 'zod';

const orderSchema = z.object({
    id: z.number().positive(),  
	description: z.string(),  
	items: z.array(z.object({
	    id: z.number().positive(),    
		quantity: z.number(),    
		description: z.string(),  
	})).refine((items) => items.length > 0, {
	    message: 'Order must have at least one item',  
	}),
})  
    .refine((order) => order.id > 100 && order.items.length > 100, {
	    message:      'All orders with more than 100 items must have an id greater than 100', 
	});

Use .refine on the items field to check whether the order has at least one item. You can also combine multiple fields, such as order.id and order.items.length, to create specific rules for orders with more than 100 items. Note that .refine runs during the validation step, while .transform is applied after validation completes. This allows you to reshape the data to normalize the output.

## Conclusion
Powertools for AWS Lambda (TypeScript) introduces a new Parser utility that makes it easier to add validation to your Lambda functions.  
By leveraging the popular validation library Zod, Powertools provides a rich set of built-in schemas for AWS service integrations such as Amazon SQS, Amazon DynamoDB, and Amazon EventBridge. Developers can use these schemas to validate event payloads and customize them to meet their business needs.

Visit the documentation to learn more and join the Powertools community Discord to connect with fellow serverless enthusiasts.
