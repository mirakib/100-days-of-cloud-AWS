# Day 28: Creating a Private ECR Repository

They need to create a private **Amazon Elastic Container Registry (ECR)** repository to store their Docker images. Once the repository is created, they will build a **Docker image** from a **Dockerfile** located on the `aws-client` host and push this image to the **ECR repository**. This process is essential for maintaining and deploying containerized applications in a streamlined manner.

Create a private ECR repository named `nautilus-ecr`. There is a Dockerfile under `/root/pyapp` directory on `aws-client` host, build a docker image using this Dockerfile and push the same to the newly created ECR repo, the image tag must be `latest`.


## To create a Amazon ECR private repository (AWS Management Console)

1. Open the Amazon ECR console at [https://console.aws.amazon.com/ecr/repositories](https://console.aws.amazon.com/ecr/repositories)

2. From the navigation bar, choose the `Region` to create your repository in.

3. Choose `Private repositories`, and then choose `Create repository`.

   <img width="1346" height="314" alt="image" src="https://github.com/user-attachments/assets/41cb9ab2-4492-49ed-820b-cf6f1f9f526c" />

5. For R`epository name`, enter a unique name for your repository. The repository name can be specified on its own (for example `nginx-web-app`). Alternatively, it can be prepended with a namespace to group the repository into a category (for example project-a/nginx-web-app).

   <img width="1299" height="166" alt="image" src="https://github.com/user-attachments/assets/8a49e52b-81dd-46c2-8274-a1f40d98575f" />
   
6. For Image tag immutability, choose one of the following tag mutability settings for the repository.

   - **Mutable** – Choose this option if you want image tags to be overwritten. 

   - **Immutable** – Choose this option if you want to prevent image tags from being overwritten.

   <img width="1299" height="300" alt="image" src="https://github.com/user-attachments/assets/c463323a-55a7-43d8-8dd0-1dce193b0040" />

7. For `Encryption configuration`, choose between `AES-256` or `AWS KMS`.

   <img width="1299" height="274" alt="image" src="https://github.com/user-attachments/assets/7bbed734-1cfa-426d-89e7-506bb156e657" />

9. Choose `Create`.

## Build docker image on azure-client

1. Navigate to directory

  ```bash
  cd pyapp
  ```

2. Build docker image

  ```bash
  docker build -t python-image .
  ```

## To push a Docker image to an Amazon ECR repository

1. Authenticate your Docker client to the Amazon ECR registry to which you intend to push your image.

   ```bash
   aws ecr get-login-password --region <region> | docker login --username AWS --password-stdin <aws_account_id>.dkr.ecr.<region>.amazonaws.com
   ```
   Example:
   
   ```bash
   aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 530847019760.dkr.ecr.us-east-1.amazonaws.com
   ```

1. Tag your image with the Amazon ECR registry, repository, and optional image tag name combination to use. The registry format is `aws_account_id.dkr.ecr.region.amazonaws.com`. The repository name should match the repository that you created for your image. If you omit the image tag, we assume that the tag is `latest`.

   ```bash
    docker tag python-image:latest 530847019760.dkr.ecr.us-east-1.amazonaws.com/nautilus-ecr:latest
    ```
2. Push the image using the docker push command:

   ```bash
   docker push 530847019760.dkr.ecr.us-east-1.amazonaws.com/nautilus-ecr:latest
   ```
   

## (Optional) To create a repository (AWS CLI)

You can create a repository using the AWS CLI with the `aws ecr create-repository` command.

```bash
aws ecr create-repository \
            --repository-name nautilus-ecr \
            --region us-east-1
```


