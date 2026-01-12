# Day 47: Integrating AWS SQS and SNS for Reliable Messaging

The Nautilus DevOps team needs to implement priority queuing using Amazon SQS and SNS. The goal is to create a system where messages with different priorities are handled accordingly. You are required to use AWS CloudFormation to deploy the necessary resources in your AWS account. The CloudFormation template should be created on the AWS client host at `/root/devops-priority-stack.yml`, the stack name must be `devops-priority-stack` and it should create the following resources:

- Two SQS queues named `devops-High-Priority-Queue` and `devops-Low-Priority-Queue`.
- An SNS topic named `devops-Priority-Queues-Topic`.
- A Lambda function named devops-priorities-queue-function that will consume messages from the SQS queues. The Lambda function code is provided in `/root/index.py` on the AWS client host.
- An IAM role named `lambda_execution_role` that provides the necessary permissions for the Lambda function to interact with SQS and SNS.

Once the stack is deployed, to test the same you can publish messages to the SNS topic, invoke the Lambda function and observe the order in which they are processed by the Lambda function. The high-priority message must be processed first.


## Task 1: Create IAM role

1. **AWS Console → IAM → Roles → Create role**

   <img width="1015" height="350" alt="image" src="https://github.com/user-attachments/assets/2545fd22-f0fb-418d-9f36-dc2b6110cd9f" />

   <img width="1015" height="297" alt="image" src="https://github.com/user-attachments/assets/748eba10-49ef-4cc4-bfb3-ab7b9b7faf49" />

   <img width="1015" height="181" alt="image" src="https://github.com/user-attachments/assets/b949f720-a9c5-4627-9689-0d39fbe3f780" />

2. **Open the role → copy Role ARN**

   <img width="1018" height="177" alt="image" src="https://github.com/user-attachments/assets/860da0df-2bcf-48e3-98c4-af91d798bc6c" />

## Task 1: Create the CloudFormation template

**On the AWS client host, create the file exactly here:**

```sh
vi /root/devops-priority-stack.yml
```

>[!Note]
> **Must replace ARN**

**`devops-priority-stack.yml`**

```yml
AWSTemplateFormatVersion: '2010-09-09'
Description: Priority Queuing using SNS, SQS and Lambda (Lab Compatible)

Resources:

  HighPriorityQueue:
    Type: AWS::SQS::Queue
    Properties:
      QueueName: devops-High-Priority-Queue

  LowPriorityQueue:
    Type: AWS::SQS::Queue
    Properties:
      QueueName: devops-Low-Priority-Queue

  PriorityTopic:
    Type: AWS::SNS::Topic
    Properties:
      TopicName: devops-Priority-Queues-Topic

  HighQueuePolicy:
    Type: AWS::SQS::QueuePolicy
    Properties:
      Queues:
        - !Ref HighPriorityQueue
      PolicyDocument:
        Version: '2012-10-17'
        Statement:
          - Effect: Allow
            Principal: "*"
            Action: sqs:SendMessage
            Resource: !GetAtt HighPriorityQueue.Arn
            Condition:
              ArnEquals:
                aws:SourceArn: !Ref PriorityTopic

  LowQueuePolicy:
    Type: AWS::SQS::QueuePolicy
    Properties:
      Queues:
        - !Ref LowPriorityQueue
      PolicyDocument:
        Version: '2012-10-17'
        Statement:
          - Effect: Allow
            Principal: "*"
            Action: sqs:SendMessage
            Resource: !GetAtt LowPriorityQueue.Arn
            Condition:
              ArnEquals:
                aws:SourceArn: !Ref PriorityTopic

  HighPrioritySubscription:
    Type: AWS::SNS::Subscription
    DependsOn: HighQueuePolicy
    Properties:
      TopicArn: !Ref PriorityTopic
      Protocol: sqs
      Endpoint: !GetAtt HighPriorityQueue.Arn
      FilterPolicy:
        priority:
          - high

  LowPrioritySubscription:
    Type: AWS::SNS::Subscription
    DependsOn: LowQueuePolicy
    Properties:
      TopicArn: !Ref PriorityTopic
      Protocol: sqs
      Endpoint: !GetAtt LowPriorityQueue.Arn
      FilterPolicy:
        priority:
          - low

  PriorityLambda:
    Type: AWS::Lambda::Function
    Properties:
      FunctionName: devops-priorities-queue-function
      Runtime: python3.9
      Handler: index.lambda_handler
      Role: arn:aws:iam::841089144903:role/lambda_execution_role
      Code:
        ZipFile: |
          def lambda_handler(event, context):
              print(event)
      Timeout: 30

  HighPriorityEventSource:
    Type: AWS::Lambda::EventSourceMapping
    Properties:
      EventSourceArn: !GetAtt HighPriorityQueue.Arn
      FunctionName: !Ref PriorityLambda
      BatchSize: 1
      Enabled: true

  LowPriorityEventSource:
    Type: AWS::Lambda::EventSourceMapping
    Properties:
      EventSourceArn: !GetAtt LowPriorityQueue.Arn
      FunctionName: !Ref PriorityLambda
      BatchSize: 1
      Enabled: true
```

## Task 3: Deploy the stack using CLI

```sh
aws cloudformation create-stack \
  --stack-name devops-priority-stack \
  --template-body file:///root/devops-priority-stack.yml \
  --capabilities CAPABILITY_NAMED_IAM
```

**Wait until stack completes:**

```sh
aws cloudformation wait stack-create-complete \
  --stack-name devops-priority-stack
```

## Task 4: Upload real Lambda code using CLI

Your real Lambda code already exists at:

```sh
/root/index.py
```

Package and upload it:

```sh
cd /root
zip function.zip index.py
```

```sh
aws lambda update-function-code \
  --function-name devops-priorities-queue-function \
  --zip-file fileb://function.zip
```

## Task 3: Test Priority Queuing

**Get SNS Topic ARN:**

```sh
topicarn=$(aws sns list-topics \
  --query "Topics[?contains(TopicArn,'nautilus-Priority-Queues-Topic')].TopicArn" \
  --output text)
```

**Publish messages**

```sh
aws sns publish \
  --topic-arn $topicarn \
  --message 'High Priority message 1' \
  --message-attributes '{"priority":{"DataType":"String","StringValue":"high"}}'
```

```sh
aws sns publish \
  --topic-arn $topicarn \
  --message 'High Priority message 2' \
  --message-attributes '{"priority":{"DataType":"String","StringValue":"high"}}'
```

```sh
aws sns publish \
  --topic-arn $topicarn \
  --message 'Low Priority message 1' \
  --message-attributes '{"priority":{"DataType":"String","StringValue":"low"}}'
```
