# Day 47: Integrating AWS SQS and SNS for Reliable Messaging

The Nautilus DevOps team needs to implement priority queuing using Amazon SQS and SNS. The goal is to create a system where messages with different priorities are handled accordingly. You are required to use AWS CloudFormation to deploy the necessary resources in your AWS account. The CloudFormation template should be created on the AWS client host at `/root/xfusion-priority-stack.yml`, the stack name must be `xfusion-priority-stack` and it should create the following resources:

- Two SQS queues named xfusion-High-Priority-Queue and `xfusion-Low-Priority-Queue`.
- An SNS topic named `xfusion-Priority-Queues-Topic`.
- A Lambda function named `xfusion-priorities-queue-function` that will consume messages from the SQS queues. The Lambda function code is provided in `/root/index.py` on the AWS client host.
- An IAM role named `lambda_execution_role` that provides the necessary permissions for the Lambda function to interact with SQS and SNS.

Once the stack is deployed, to test the same you can publish messages to the SNS topic, invoke the Lambda function and observe the order in which they are processed by the Lambda function. The high-priority message must be processed first.


## Task 1: Create the CloudFormation template

1. **Create S3 Bucket using CLI from azure-client host:**

   ```sh
   aws s3 mb s3://kklabsuser-352734
   ```

2. **Package code in azure-client host::**

   ```sh
   zip function.zip index.py
   ```

3. **Copy zip to s3 bucket using CLI:**

   ```sh
   aws s3 cp function.zip s3://kklabsuser-352734
   ```

4. **On the AWS client host, create the file exactly here:**

   ```sh
   vi /root/devops-priority-stack.yml
   ```

   **`Look at the end of this document:`**

5. **Deploy the stack using CLI**

   ```sh
   aws cloudformation create-stack \
     --stack-name xfusion-priority-stack \
     --template-body file:///root/xfusion-priority-stack.yml \
     --capabilities CAPABILITY_NAMED_IAM
   ```

**`/root/devops-priority-stack.yml`**

```yaml
AWSTemplateFormatVersion: '2010-09-09'
Description: Priority Queue Processing Stack
Resources:
  
  HighPriorityQueue:
    Type: AWS::SQS::Queue
    Properties:
      QueueName: xfusion-High-Priority-Queue
      VisibilityTimeout: 60

  LowPriorityQueue:
    Type: AWS::SQS::Queue
    Properties:
      QueueName: xfusion-Low-Priority-Queue
      VisibilityTimeout: 60

  PriorityQueuesTopic:
    Type: AWS::SNS::Topic
    Properties:
      TopicName: xfusion-Priority-Queues-Topic

  HighPrioritySubscription:
    Type: AWS::SNS::Subscription
    Properties:
      TopicArn: !Ref PriorityQueuesTopic
      Protocol: sqs
      Endpoint: !GetAtt HighPriorityQueue.Arn
      FilterPolicy:
        priority:
          - high

  LowPrioritySubscription:
    Type: AWS::SNS::Subscription
    Properties:
      TopicArn: !Ref PriorityQueuesTopic
      Protocol: sqs
      Endpoint: !GetAtt LowPriorityQueue.Arn
      FilterPolicy:
        priority:
          - low 

  HighPriorityPolicy:
    Type: AWS::SQS::QueuePolicy
    Properties:
      Queues:
        - !Ref HighPriorityQueue
      PolicyDocument:
        Statement:
          - Effect: Allow
            Principal: "*"
            Action: SQS:SendMessage
            Resource: !GetAtt HighPriorityQueue.Arn
            Condition:
              ArnEquals:
                aws:SourceArn: !Ref PriorityQueuesTopic
                
  LowPriorityPolicy:
    Type: AWS::SQS::QueuePolicy
    Properties:
      Queues:    
        - !Ref LowPriorityQueue
      PolicyDocument:
        Statement:
          - Effect: Allow
            Principal: "*"  
            Action: SQS:SendMessage
            Resource: !GetAtt LowPriorityQueue.Arn
            Condition:
              ArnEquals:
                aws:SourceArn: !Ref PriorityQueuesTopic

  LambdaExecutionRole:
    Type: AWS::IAM::Role
    Properties:
      RoleName: lambda_execution_role
      AssumeRolePolicyDocument:
        Version: '2012-10-17'
        Statement:
          - Effect: Allow
            Principal:
              Service: lambda.amazonaws.com
            Action: sts:AssumeRole
      ManagedPolicyArns:
        - arn:aws:iam::aws:policy/service-role/AWSLambdaBasicExecutionRole
        - arn:aws:iam::aws:policy/AmazonSQSFullAccess
        - arn:aws:iam::aws:policy/AmazonSNSFullAccess

  PriorityLambdaFunction:
    Type: AWS::Lambda::Function
    Properties:
      FunctionName: xfusion-priorities-queue-function
      Runtime: python3.9
      Handler: index.lambda_handler
      Role: !GetAtt LambdaExecutionRole.Arn
      Timeout: 5
      Environment:
        Variables:
          high_priority_queue: !Ref HighPriorityQueue
          low_priority_queue: !Ref LowPriorityQueue
      Code:
        S3Bucket: kklabsuser-352734
        S3Key: function.zip
```
