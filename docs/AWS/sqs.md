# SQS
> [Decouple application](#Decouple_application)  
> [Visibility timeout](#Visibility_Timeout)


## Decouple_application
- In that case, it’s better to decouple your applications,
    - using SQS: queue model
    - using SNS: pub/sub model (publish / subscribe)
    - using Kinesis: real-time streaming model

## Visibility_Timeout
- When a consumer receives and processes a message from a queue, the message remains in the queue. Amazon SQS doesn't automatically delete the message. Because Amazon SQS is a distributed system, there's no guarantee that the consumer actually receives the message (for example, due to a connectivity issue, or due to an issue in the consumer application). Thus, the consumer must delete the message from the queue after receiving and processing it.

- Immediately after a message is received, it remains in the queue. **To prevent other consumers from processing the message again, Amazon SQS sets a visibility timeout, a period of time during which Amazon SQS prevents other consumers from receiving and processing the message.** The default visibility timeout for a message is 30 seconds. 

