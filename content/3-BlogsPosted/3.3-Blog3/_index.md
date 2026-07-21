---
title: "Blog 3"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 3.3. </b> "
---

# A SERVERLESS IMAGE PROCESSING PIPELINE WITH S3, LAMBDA, DYNAMODB AND SNS

Image processing on traditional servers (resizing and watermarking at upload time) has a familiar weakness: when traffic spikes, the server becomes sluggish or even stops responding entirely. The whole processing load lands on a fixed process that cannot scale with demand.

This post shares a serverless image processing pipeline on AWS, built with the S3 – Lambda – DynamoDB – SNS service set, which runs smoothly and scales automatically with traffic.

**The role of each service**

* **S3**: Serves both as storage for original and processed images, and as the event source (`s3:ObjectCreated`) that triggers Lambda whenever a new image is uploaded.
* **Lambda**: Acts as the central processing "hub" — after resizing and watermarking, Lambda actively writes results to S3, DynamoDB, and SNS in parallel, rather than calling each step sequentially.
* **DynamoDB**: Stores metadata such as processing status and image URLs, which the client can poll through API Gateway to check progress.
* **SNS**: Pushes completion notifications to registered subscribers (mobile apps via push notification, email, etc.), running in parallel with the polling flow so clients do not have to wait passively.

**Three lessons worth remembering**

* Never write processed images back into the same bucket that triggered the function — it creates an infinite loop immediately. Separating into two buckets (`raw-images` / `processed-images`) or using distinct prefixes is the safe solution.
* Lambda is a hub, not a link in a chain — Lambda itself calls S3, DynamoDB, and SNS independently; it is not S3 calling DynamoDB, which then calls SNS.
* Poll (via API Gateway) and Push (via SNS) are two completely separate flows — do not draw them as one or conflate them, or the architecture becomes confusing to debug.

The S3 → Lambda → DynamoDB/SNS → API Gateway pattern fits asynchronous file processing well and scales cleanly. The hard part is not remembering service names, but understanding exactly "who calls whom" to avoid infinite loops and unnecessary costs.

![Serverless Image Processing Pipeline](/images/3-BlogsPosted/3.3-Blog3/ServerlessImagePipeline.jpg)

References for readers who want to dive deeper:

* [Using AWS Lambda with Amazon S3](https://docs.aws.amazon.com/lambda/latest/dg/with-s3.html)
* [Amazon S3 Event Notifications](https://docs.aws.amazon.com/AmazonS3/latest/userguide/EventNotifications.html)
* [Introduction to Amazon DynamoDB](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Introduction.html)
* [Amazon API Gateway REST API](https://docs.aws.amazon.com/apigateway/latest/developerguide/apigateway-rest-api.html)
* [Amazon SNS Developer Guide](https://docs.aws.amazon.com/sns/latest/dg/welcome.html)

[Link to the original post](https://www.facebook.com/share/p/1BvwKj9juC/)
