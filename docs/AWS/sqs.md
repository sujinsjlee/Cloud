# SQS
> [Decouple application](#Decouple-application)  
> [Visibility timeout](#Visibility-Timeout)  
> [DLQ](#DLQ)  
> [Temporary Queue](#Temporary-Queue)  
> [Priority](#Priority)  
> [FIFO](#FIFO)  

## Decouple application
- In that case, it’s better to decouple your applications,
    - using SQS: queue model
    - using SNS: pub/sub model (publish / subscribe)
    - using Kinesis: real-time streaming model

## Visibility Timeout
- When a consumer receives and processes a message from a queue, the message remains in the queue. Amazon SQS doesn't automatically delete the message. Because Amazon SQS is a distributed system, there's no guarantee that the consumer actually receives the message (for example, due to a connectivity issue, or due to an issue in the consumer application). Thus, the consumer must delete the message from the queue after receiving and processing it.

- Immediately after a message is received, it remains in the queue. **To prevent other consumers from processing the message again, Amazon SQS sets a visibility timeout, a period of time during which Amazon SQS prevents other consumers from receiving and processing the message.** The default visibility timeout for a message is 30 seconds. 

## DLQ
- If a consumer fails to process a message within the Visibility Timeout… the message goes back to the queue!
- Useful for **debugging!**

## Temporary Queue
- SQS – Request-Response Systems
- To implement this pattern: use the SQS Temporary Queue Client
    - The most common use case for temporary queues is the request-response messaging pattern, where a requester creates a temporary queue for receiving each response message. To avoid creating an Amazon SQS queue for each response message, the Temporary Queue Client lets you create and delete multiple temporary queues without making any Amazon SQS API calls.

## Priority
> **Priority: Use separate queues to provide prioritization of work.**  
    - [Amazon SQS features](https://aws.amazon.com/sqs/features)

## FIFO
- Use Amazon Simple Queue Service (Amazon SQS) FIFO queues for capturing the action and **draining the queue**

## Attributes
- Default retention of messages: 4 days, maximum of 14 days
- Low latency (<10 ms on publish and receive)
- Limitation of **256KB** per message sent // TB단위의 msg handling 불가능