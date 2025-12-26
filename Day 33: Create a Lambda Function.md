# Day 33: Create a Lambda Function


- **Create Lambda Function**: Create a Lambda function named `xfusion-lambda`.
- **Runtime**: Use the Runtime `Python`.
- **Deploy**: The function should print the body `Welcome to KKE AWS Labs!`.
- **Status Code**: Ensure the status code is `200`.
- **IAM Role**: Create and use the IAM role named `lambda_execution_role`.


## Create and use the IAM role named `lambda_execution_role`.

1. Go to IAM dashboard and select `Role` from navigation pane.

   <img width="233" height="426" alt="image" src="https://github.com/user-attachments/assets/bec39abb-a30c-45ba-a8c2-bd8ad629e298" />

2. From role pane, click on `Create role`
3. Under `Trusted entity type`, select `Aws service`.

   <img width="1049" height="248" alt="image" src="https://github.com/user-attachments/assets/96bed990-a87c-4a7f-8466-580a9d695db6" />

4. Under `Use case`, select `Lambda` and click `Next`.

   <img width="1049" height="264" alt="image" src="https://github.com/user-attachments/assets/6236f6a0-3946-4cea-9864-3f784b6406d6" />

5. On `Step 2`, select desired policy and click `Next`.

   <img width="1049" height="263" alt="image" src="https://github.com/user-attachments/assets/4484436e-c4f6-4329-aa83-777389fa4d23" />

6. On `Step 3`, enter role name and click on `Create role`.

   <img width="1049" height="282" alt="image" src="https://github.com/user-attachments/assets/00231078-0e0f-4763-a034-71e4501abe62" />
   
## To create a Lambda function with the console

1. Open the Functions page of the Lambda console.

   <img width="1348" height="315" alt="image" src="https://github.com/user-attachments/assets/af4c6735-fcc4-4117-9a7a-4358c7413a76" />

3. Choose **Create function**.

   <img width="317" height="180" alt="image" src="https://github.com/user-attachments/assets/2a573063-fd0e-4800-9202-1abfcb6e08ad" />

5. Select **Author from scratch**.

   <img width="1348" height="151" alt="image" src="https://github.com/user-attachments/assets/eb29a2ca-f542-4396-a835-cbfdd00473ce" />

7. In the **Basic information** pane, for **Function name**.

   <img width="1303" height="142" alt="image" src="https://github.com/user-attachments/assets/56da6b7a-7f34-4b95-bff0-8bdf467edf6b" />

9. For **Runtime**, choose either `Node.js 24` or `Python 3.14`.

    <img width="1303" height="85" alt="image" src="https://github.com/user-attachments/assets/ae93b935-6013-40c6-b821-9e85adf3ff29" />

10. Leave architecture set to `x86_64`, and then choose `Create function`.

    <img width="1303" height="110" alt="image" src="https://github.com/user-attachments/assets/83672a1f-3242-43af-9a60-2934de2d53af" />

10. On `Permission` pane, click on `Change default execution role` and select `Use existing role` to choose `lambda_execution_role` under `Existing role`.

    <img width="1303" height="317" alt="image" src="https://github.com/user-attachments/assets/f6ba14f5-46cf-45f6-91ce-6ebcd36bac54" />

12. After creating the function, under the `Code source` edit the `lambda_function.py` with following code.

    ```python
    import json

    def lambda_handler(event, context):
      # TODO implement
      return {
        'statusCode': 200,
        'body': json.dumps('Welcome to KKE AWS Labs!')
      }
    ```

13. Click on `Deploy` from `Code source` section.

    <img width="310" height="374" alt="image" src="https://github.com/user-attachments/assets/0fd6c1c9-d91c-4be3-ae02-8d3bec8837b2" />

14. In the `Test` section of the console code editor, choose `Create test event` with a test name..

    <img width="492" height="228" alt="image" src="https://github.com/user-attachments/assets/5cf14e08-6bc9-450e-b4a6-4b15eec31b83" />

12. Click on `Invoke` and see output in putput section.

    <img width="976" height="173" alt="image" src="https://github.com/user-attachments/assets/32edc811-49e0-4e33-87ce-b91f94499529" />

