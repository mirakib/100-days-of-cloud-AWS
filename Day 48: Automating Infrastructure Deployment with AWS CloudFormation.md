# Day 48: Automating Infrastructure Deployment with AWS CloudFormation

The Nautilus DevOps team needs to implement a Lambda function using a CloudFormation stack. Create a CloudFormation template named `/root/datacenter-lambda.yml` on the AWS client host and configure it to create the following components. The stack name must be `datacenter-lambda-app`.

- Create a Lambda function named `datacenter-lambda`.
- Use the Runtime `Python`.
- The function should print the body `Welcome to KKE AWS Labs!`.
- Ensure the status code is `200`.
- Create and use the IAM role named `lambda_execution_role`.

1. **On the AWS client host, create the file exactly here:**

   ```sh
   vi /root/datacenter-lambda.yml
   ```

   **`Look at the end of this document:`**

2. **Deploy the stack using CLI**

   ```sh
   aws cloudformation create-stack \
     --stack-name datacenter-lambda-app \
     --template-body file:///root/datacenter-lambda.yml \
     --capabilities CAPABILITY_NAMED_IAM
   ```

3. **Invoke the lambda function using CLI:**

   ```sh
   aws lambda invoke --function-name datacenter-lambda output.json
   ```

   **Expected:**

   ```json
   {
    "StatusCode": 200,
    "ExecutedVersion": "$LATEST"
   }
   ```

5. **Verify lambda invocation returns expected message:**

   ```sh
   cat output.json
   ```

   **Expected:**

   ```json
   {"statusCode": 200, "body": "Welcome to KKE AWS Labs!"}
   ```


<div align="center">
<h3> /root/datacenter-lambda.yml </h3>
</div>

```yml
AWSTemplateFormatVersion: '2010-09-09'
Description: Lambda function that returns a welcome message
Resources:
  
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

  LambdaFunction:
    Type: AWS::Lambda::Function
    Properties:
      FunctionName: datacenter-lambda
      Runtime: python3.9
      Handler: index.lambda_handler
      Role: !GetAtt LambdaExecutionRole.Arn
      Timeout: 5
      Code:
        ZipFile: |
          def lambda_handler(event, context):
              message = "Welcome to KKE AWS Labs!"
              return {
              "statusCode": 200,
               "body": message
               }
```
