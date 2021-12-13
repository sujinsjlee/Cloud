# AWS Lambda
> [AWS Lambda permission](#AWS_Lambda_permission)  


## AWS_Lambda_permission
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

