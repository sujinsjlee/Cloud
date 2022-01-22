# AWS Lambda
> [AWS Lambda permission](#AWS-Lambda-permission)  
> [Lambda max 15min](#max-15)  
> [AWS Step Function](#AWS-Step-Function)  
> [Lambda@Edge](#Lambda@Edge)  
> [EventBridge](#EventBridge)  

## AWS Lambda permission
- [Using resource-based policies for Amazon EventBridge](https://docs.aws.amazon.com/eventbridge/latest/userguide/eb-use-resource-based.html#lambda-permissions)
- To invoke your AWS Lambda function by using **a EventBridge rule**, add the following permission to the policy of your **Lambda function**.

    ```json
    {
    "Effect": "Allow",
    "Action": "lambda:InvokeFunction",
    "Resource": "arn:aws:lambda:region:account-id:function:function-name",
    "Principal": {
        "Service": "events.amazonaws.com"
    },
    "Condition": {
        "ArnLike": {
        "AWS:SourceArn": "arn:aws:events:region:account-id:rule/rule-name"
        }
    },
    "Sid": "InvokeLambdaFunction"
    }
    ```

## max 15
- [15min](https://aws.amazon.com/lambda/faqs/#:~:text=AWS%20Lambda%20functions%20can%20be,1%20second%20and%2015%20minutes.)
    - *Q: How long can an AWS Lambda function execute?*
    - AWS Lambda functions can be configured to run up to **15 minutes** per execution. You can set the timeout to any value between 1 second and 15 minutes.

## AWS Step Function
- Build serverless visual workflow to orchestrate your Lambda functions

- Q: How do I coordinate calls between multiple AWS Lambda functions?

    - You can use AWS Step Functions to coordinate a series of AWS Lambda functions in a specific order. You can invoke multiple Lambda functions sequentially, passing the output of one to the other, and/or in parallel, and Step Functions will maintain state during executions for you.


## Lambda@Edge
- Q: AWS Lambda@Edge는 Amazon API Gateway 뒤에서 AWS Lambda를 사용하는 것과 어떻게 다릅니까?

- 다른 점은 API Gateway와 Lambda가 리전별 서비스라는 것입니다. Lambda@Edge 및 Amazon CloudFront를 사용하면 최종 사용자의 위치에 따라 여러 AWS 위치에서 로직을 실행할 수 있습니다.

- Lambda@Edge is optimized for **latency-sensitive use cases** where your end viewers are distributed globally. All the information you need to make a decision should be available at the CloudFront edge, within the function and the request. This means that use cases where you are looking to make decisions on how to serve content based on user characteristics (e.g., location, client device, etc.) can now be executed and served close to your users without having to be routed back to a centralized server.

## EventBridge
- [Schedule AWS Lambda functions using EventBridge](https://docs.aws.amazon.com/ko_kr/eventbridge/latest/userguide/eb-run-lambda-schedule.html)  


## Test Tips
- "scalable and elastic" -> serverless

## Lambda Features
- Virtual functions – no servers to manage!
- Limited by time - **short executions**
- Run **on-demand**
- Scaling is **automated!**(in lambda, scaling is not to enable/disable)
    - cf) known in advance environmant --> doesn't need AutoScale