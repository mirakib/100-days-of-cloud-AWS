# Day 34: Create a Lambda Function Using CLI

The Nautilus DevOps team continues to explore serverless architecture by setting up another Lambda function. This time, the task must be completed using the AWS Console to familiarize the team with the web interface. The function will return a custom greeting and demonstrate the capabilities of AWS Lambda effectively.

- **Create Python Script**: Create a **Python script** named `lambda_function.py` with a function that returns the body `Welcome to KKE AWS Labs!` and status code `200`.

- **Zip the Python Script**: **Zip the script** into a file named `function.zip`.

- **Create Lambda Function**: Create a Lambda function named `datacenter-lambda-cli` using the zipped file and specify `Python` as the runtime.

- **IAM Role**: Use the **IAM role** named `lambda_execution_role`.

## Step 1: Create the Python script `lambda_function.py`

```py
def lambda_handler(event, context):
    return {
        "statusCode": 200,
        "body": "Welcome to KKE AWS Labs!"
    }
```

## Step 2: Zip the script

```bash
zip function.zip lambda_function.py
```

## Step 3: Retrive AWS account ID

```sh
aws sts get-caller-identity
```

Expected output:

```json
{
    "UserId": "AIDAWJFSHFGJK5LBXTVSB",
    "Account": "432017975698",
    "Arn": "arn:aws:iam::432017975698:user/kk_labs_user_627453"
}
```

## Step 4: Create the Lambda function using AWS CLI

```sh
aws lambda create-function \
  --function-name datacenter-lambda-cli \
  --runtime python3.9 \
  --role arn:aws:iam::<ACCOUNT_ID>:role/lambda_execution_role \
  --handler lambda_function.lambda_handler \
  --zip-file fileb://function.zip
```

## Step 5: Verify Lambda function

```sh
aws lambda get-function \
  --function-name datacenter-lambda-cli
```

## Step 6: Invoke the function

```sh
aws lambda invoke \
  --function-name datacenter-lambda-cli \
  response.json
```

Expected output:

```json
{
  "statusCode": 200,
  "body": "Welcome to KKE AWS Labs!"
}
```
