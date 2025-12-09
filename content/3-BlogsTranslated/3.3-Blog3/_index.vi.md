---
title: "Blog 3"
date: "`r Sys.Date()`"
weight: 1
chapter: false
pre: " <b> 3.3. </b> "
---

{{% notice warning %}}
⚠️ **Lưu ý:** Các thông tin dưới đây chỉ nhằm mục đích tham khảo, vui lòng **không sao chép nguyên văn** cho bài báo cáo của bạn kể cả warning này.
{{% /notice %}}

# Xác thực event payload với Powertools for AWS Lambda (TypeScript)
Trong bài viết này, hãy tìm hiểu cách Powertools for AWS Lambda (TypeScript) với Parser utility mới có thể giúp bạn xác thực (validate) payloads một cách dễ dàng và làm cho Lambda function của bạn trở nên chống chịu tốt hơn hơn.
Việc validate input payloads là một khía cạnh quan trọng trong việc xây dựng các ứng dụng secure và reliable. Điều này đảm bảo rằng dữ liệu mà một ứng dụng nhận được có thể xử lý trơn tru các unexpected hoặc malicious inputs và ngăn chặn harmful downstream processing. Khi viết AWS Lambda functions, các developer cần validate và verify payload, đồng thời đảm bảo rằng các fields và values cụ thể là chính xác và an toàn để xử lý.

Powertools for AWS Lambda là một developer toolkit có sẵn cho Python, NodeJS/TypeScript, Java và .NET. Nó giúp triển khai serverless best practices và tăng tốc độ phát triển (developer velocity). Powertools for AWS Lambda (TypeScript) hiện đang giới thiệu một Parser utility mới để giúp các developer dễ dàng hơn trong việc triển khai validation trong Lambda functions của họ.

## Tại sao việc payload validation lại quan trọng
Validating payloads có thể giúp Lambda functions của bạn trở nên resilient hơn. Payloads kết hợp cả technical và business information cũng có thể gây khó khăn trong việc validate. Điều này đòi hỏi phải viết validation logic bên trong Lambda function code của bạn. Việc này có thể dao động từ một vài if-statements để kiểm tra payload values, cho đến một chuỗi các validation steps phức tạp dựa trên custom business logic. Bạn có thể cần phải tách riêng việc validation của technical information trong payload như AWS Region, accountId, event source, và business information bên trong event như productId và payment details.

Việc hiểu rõ structure và values của event object, cũng như cách extract relevant information, có thể rất thách thức. Ví dụ, một Amazon SQS event có một body field với string value, có thể là một JSON document. Amazon EventBridge có một object trong detail field mà bạn có thể đọc trực tiếp mà không cần transformation thêm. Bạn có thể cần phải decompress, decode, transform, và validate payload bên trong một specific field. Việc hiểu rõ nhiều transformation layers có thể trở nên phức tạp, đặc biệt nếu event object của bạn là kết quả của nhiều service invocations.


## Sử dụng Powertools for AWS Lambda (TypeScript) Parser Utility
Powertools for AWS Lambda (TypeScript) là một modular library. Bạn có thể cài đặt có chọn lọc các features như Logger, Tracer, Metrics, Batch Processing, Idempotency, và nhiều hơn nữa. Bạn có thể sử dụng Powertools for AWS Lambda trong cả TypeScript và JavaScript code bases. Parser utility mới này giúp đơn giản hóa việc validation và sử dụng validation library phổ biến là Zod.
Bạn có thể sử dụng parser như một method decorator, với middyjs middleware, hoặc manually trong tất cả các Lambda provided NodeJS runtimes.

Để sử dụng utility, hãy install Powertools parser utility và Zod (<v3.x) bằng NPM hoặc bất kỳ package manager nào mà bạn chọn:
npm install @aws-lambda-powertools/parser zod@~3
Bạn có thể định nghĩa schema của mình bằng cách sử dụng Zod. Dưới đây là ví dụ về một simple order schema để validate events:
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
Schema sau đây định nghĩa id, description, và một list of items. Bạn có thể chỉ định value types từ simple numeric, thu hẹp lại thành positive hoặc literal, hoặc phức tạp hơn như union, array, hoặc thậm chí other schema. Zod cung cấp một danh sách phong phú các value types mà bạn có thể sử dụng.

Thêm parser decorator vào handler function của bạn, đặt schema parameter, và sử dụng schema này để parse event object.
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
Lưu ý rằng z.infer giúp extract Order type từ schema, điều này cải thiện development experience với autocomplete khi sử dụng TypeScript. Zod sẽ parse entire object, bao gồm cả nested fields, và báo cáo tất cả errors kết hợp, thay vì chỉ trả về error đầu tiên.


## Sử dụng built-in schema cho AWS services
Một trường hợp phổ biến hơn là cần validate events từ các AWS Services kích hoạt Lambda functions, bao gồm Amazon SQS, Amazon EventBridge, và nhiều dịch vụ khác. Để việc này dễ dàng hơn, Powertools bao gồm các pre-built schema cho các AWS events mà bạn có thể sử dụng.

Để parse một Amazon EventBridge event đến, hãy thiết lập built-in schema trong cấu hình parser của bạn:

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

Event object được parsed và validated trong runtime, và TypeScript type EventBridgeEvent giúp bạn hiểu cấu trúc và truy cập các fields trong quá trình phát triển. Trong ví dụ này, bạn chỉ parse EventBridge event object, vì vậy detail field có thể là một arbitrary object.
Bạn cũng có thể extend built-in EventBridge schema và override detail field bằng custom orderSchema của bạn:

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

Parser sẽ validate toàn bộ cấu trúc của EventBridge event, bao gồm custom business object.
Sử dụng .extend hoặc các Zod schema functions khác để thay đổi bất kỳ field nào của built-in schema và customize payload validation.


## Sử dụng envelopes với custom schema
Trong một số trường hợp, bạn chỉ cần phần custom của payload, ví dụ detail field của EventBridge event hoặc body của SQS records. Việc này yêu cầu bạn parse event schema thủ công, extract field cần thiết, rồi parse lại với custom schema. Điều này phức tạp vì bạn phải biết chính xác payload field nào cần xử lý và cách transform & parse nó.

Powertools Parser utility giúp giải quyết vấn đề này bằng Envelopes.Envelopes là các schema objects có sẵn logic để extract custom payloads.

Dưới đây là ví dụ về EventBridgeEnvelope và cách hoạt động của nó:
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

Bằng cách thiết lập schema và envelope, parser utility sẽ biết cách kết hợp cả hai tham số, extract và validate custom payload từ event.
Powertools Parser transform event object theo schema definition, cho phép bạn tập trung vào phần business-critical trong handler function.



## Safe parsing
Nếu object không khớp với Zod schema được cung cấp, theo mặc định, parser sẽ ném ra ParserError.Nếu bạn cần kiểm soát validation errors và muốn triển khai custom error handling, hãy sử dụng tùy chọn safeParse.

Dưới đây là ví dụ về cách capture failed validations như một metric trong handler function của bạn:
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

Khi đặt safeParse = true, parser sẽ không ném lỗi, mà sẽ trả về event object đã được sửa đổi với success flag và các fields error hoặc data tùy theo kết quả validation.
Bạn có thể tạo custom error handling, ví dụ tăng metric InvalidPayload và truy cập originalEvent để ghi log lỗi.
Với các validations thành công, bạn có thể truy cập data field và xử lý payload.
Lưu ý rằng event object type bây giờ là ParsedResult, với EventBridgeEvent là input và Order là output type.

## Custom validations
Đôi khi bạn có thể cần các business rules phức tạp hơn cho quá trình validation.
 Vì Parser built-in schemas là các Zod objects, bạn có thể customize validation bằng cách áp dụng .extend, .refine, .transform, và các Zod operators khác.

 Dưới đây là ví dụ về các complex rules cho orderSchema:
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

Sử dụng .refine trên items field để kiểm tra xem order có ít nhất một item hay không. Bạn cũng có thể kết hợp nhiều fields, ví dụ order.id và order.items.length, để tạo quy tắc cụ thể cho các orders có hơn 100 items. Lưu ý rằng .refine chạy trong bước validation, còn .transform sẽ được áp dụng sau khi validation hoàn tất. Điều này cho phép bạn thay đổi hình dạng dữ liệu để normalize output.

## Conclusion
Powertools for AWS Lambda (TypeScript) giới thiệu Parser utility mới giúp việc thêm validation vào Lambda functions trở nên dễ dàng hơn.
Bằng cách dựa vào validation library Zod phổ biến, Powertools cung cấp một bộ built-in schemas phong phú cho các AWS service integrations như Amazon SQS, Amazon DynamoDB, Amazon EventBridge. Các developers có thể sử dụng các schemas này để validate event payloads và customize chúng theo nhu cầu business của mình.

Hãy truy cập documentation để tìm hiểu thêm và tham gia Powertools community Discord để kết nối với các serverless enthusiasts có cùng đam mê.


