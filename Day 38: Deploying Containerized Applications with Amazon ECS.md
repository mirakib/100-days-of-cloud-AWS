# Day 38: Deploying Containerized Applications with Amazon ECS

The Nautilus DevOps team is tasked with deploying a containerized application using Amazon's container services. They need to create a private Amazon Elastic Container Registry (ECR) to store their Docker images and use Amazon Elastic Container Service (ECS) to deploy the application. The process involves building a Docker image from a given Dockerfile, pushing it to the ECR, and then setting up an ECS cluster to run the application.

**Create a Private ECR Repository**:
- Create a private ECR repository named `nautilus-ecr` to store Docker images.

**Build and Push Docker Image**:
- Use the `Dockerfile` located at `/root/pyapp` on the `aws-client` host.
- Build a Docker image using this `Dockerfile`.
- Tag the image with `latest` tag.
- Push the Docker image to the `nautilus-ecr` repository.

**Create and Configure ECS cluster**:
- Create an ECS cluster named `nautilus-cluster` using the `Fargate` launch type.

**Create an ECS Task Definition**:
- Define a task named `nautilus-taskdefinition` using the Docker image from the `nautilus-ecr` ECR repository.
- Specify necessary CPU and memory resources.

**Deploy the Application Using ECS Service**:
- Create a service named `nautilus-service` on the `nautilus-cluster` to run the task.
- Ensure the service runs at least one task.


## Task 1: Create a Private ECR Repository

1. Open the Amazon ECR console at [https://console.aws.amazon.com/ecr/repositories](https://console.aws.amazon.com/ecr/repositories).
2. From the navigation bar, Choose `Private repositories`, and then choose `Create repository`.
   
   <img width="263" height="277" alt="image" src="https://github.com/user-attachments/assets/01057a8b-1573-4aa7-a948-8f896b8f73a3" />

3. For Repository name, enter a unique name for your repository.

   <img width="1299" height="167" alt="image" src="https://github.com/user-attachments/assets/c6b4d3e0-6045-49ee-bf93-2b4ff8d5125c" />

4. Choose **Create**.

## Task 2: Build and Push Docker Image

1. Authenticate your Docker client to the Amazon ECR registry.

   ```sh
   aws ecr get-login-password --region <region> | docker login --username AWS --password-stdin <aws_account_id>.dkr.ecr.<region>.amazonaws.com
   ```

   ```sh
   aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 399513078200.dkr.ecr.us-east-1.amazonaws.com
   ```

2. Build docker image on aws-client host.

   ```sh
   cd pyapp
   ```

   ```sh
   docker build -t nautilus-ecr:latest .
   ```

3. Tag the Docker image

   ```sh
   docker tag nautilus-ecr:latest <aws_account_id>.dkr.ecr.<region>.amazonaws.com/nautilus-ecr:latest
   ```

   ```sh
   docker tag nautilus-ecr:latest 399513078200.dkr.ecr.us-east-1.amazonaws.com/nautilus-ecr:latest
   ```
   
3. Push image to ECR

   ```sh
   docker push <aws_account_id>.dkr.ecr.<region>.amazonaws.com/nautilus-ecr:latest
   ```

   ```sh
   docker push 399513078200.dkr.ecr.us-east-1.amazonaws.com/nautilus-ecr:latest
   ```

## Task 3: Create and Configure ECS cluster

1. Open the Amazon ECS console and choose `Cluster`.

   <img width="263" height="337" alt="image" src="https://github.com/user-attachments/assets/7d0f1318-084c-4c47-9ceb-0bdea5a87f7d" />

2. Click on `Create` and under `Cluster configuration` enter cluster name `nautilus-cluster`

   <img width="1018" height="200" alt="image" src="https://github.com/user-attachments/assets/b1d3f0bd-7e3c-4efb-9220-fb3e6f443127" />

3. Next on the `Infrastructure` pane, select `Farget only`.

   <img width="1018" height="243" alt="image" src="https://github.com/user-attachments/assets/89753f5c-74b8-49d6-82be-9f78486d3dd0" />

4. Choose `Create`.

## Task 4: Create an ECS Task Definition

1. From the Amazon ECS dashboard navigation pane, select `Task Defination`.

   <img width="263" height="227" alt="image" src="https://github.com/user-attachments/assets/c6d0cf6e-9f83-4d9f-9009-e9bcc5a62f28" />

2. Click `create new task defination`.

   <img width="1078" height="213" alt="image" src="https://github.com/user-attachments/assets/127ef2f4-4278-4133-b0b8-87b9c0baf8db" />

3. Under `Task defination configuration`, enter name `nautilus-taskdefinition`.

   <img width="1018" height="163" alt="image" src="https://github.com/user-attachments/assets/9278f45e-9d36-4a5c-b566-c28bed40af30" />

4. Next under `Infrastructure requirments`, select `AWS Farget`.

   <img width="1018" height="275" alt="image" src="https://github.com/user-attachments/assets/90a2715e-ddc5-4ab6-9290-98fe9f59cb63" />

5. Also OS and task size of your choice,

   <img width="1018" height="239" alt="image" src="https://github.com/user-attachments/assets/ace97510-9bf3-44cf-99bb-464f486e5997" />

6. Next browse Docker image from the nautilus-ecr ECR repository using `Browse ECR images` button.

   <img width="1018" height="276" alt="image" src="https://github.com/user-attachments/assets/ab39f9c8-e187-4581-a4f2-49ec3d586b99" />

7. Leave every other options as default and click on `Create`

8. Review the task defination after creation.

   <img width="1056" height="368" alt="image" src="https://github.com/user-attachments/assets/95f15731-da5e-4ce1-b195-b6c47e8a3247" />


## Task 5: Deploy the Application Using ECS Service

Create a service named `nautilus-service` on the `nautilus-cluster` to run the task.

1. Go to `Clusters` from navigation pane of Amazon ECS.
2. On the `Service` tab, click `Create`.

   <img width="1047" height="391" alt="image" src="https://github.com/user-attachments/assets/e08a3373-d6f7-493a-94b4-7254ec644f52" />

3. On the `Service details` pane, select task defination and enter service name.

   <img width="1018" height="348" alt="image" src="https://github.com/user-attachments/assets/6c8d0610-b3b4-42e7-ae6a-748b0e263b2d" />

4. On the `Environment` pane, select `Farget` as launch type.

   <img width="1018" height="403" alt="image" src="https://github.com/user-attachments/assets/8c386741-f6b2-40b0-bd87-c2dd9226c8af" />

5. Choose default vpc and subnets of choice and leave other options as default.
6. Click `Create`

## Task 6: verification application access

1. CLick on the service and make sure the `Status` is `Active` with 1 running task under `Service overview` tab.

   <img width="1018" height="114" alt="image" src="https://github.com/user-attachments/assets/9ed56a7f-7d5c-42e5-98c3-482f47ecebb5" />

2. Go to VPC Dashboard and change `default` security group with an `HTTP` inbound rule for `0.0.0.0/0`.

   <img width="1315" height="265" alt="image" src="https://github.com/user-attachments/assets/bf1060ea-b986-434e-9b6a-df7ad7f4afa7" />

3. Now from ECS Dashboard `Clusters > nautilus-cluster > Services > nautilus-service > Tasks`.

   <img width="1329" height="285" alt="image" src="https://github.com/user-attachments/assets/82648c4e-d752-4d38-8815-65dc74518d86" />

4. Click on the running task shown. Enter the public IP address to a browser from `Configuration` pane.

   <img width="1018" height="261" alt="image" src="https://github.com/user-attachments/assets/4505481c-8841-41b0-a19c-56432b3d42ef" />

5. In the browser, you see `Welcome to KKE AWS cloud labs!` text in a web page.

   <img width="845" height="168" alt="image" src="https://github.com/user-attachments/assets/ebbb7fa3-5fc2-415f-955d-82301010be35" />
