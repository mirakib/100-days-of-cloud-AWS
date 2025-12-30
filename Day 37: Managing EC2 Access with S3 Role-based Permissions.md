# Day 37: Managing EC2 Access with S3 Role-based Permissions

The Nautilus DevOps team needs to set up an application on an EC2 instance to interact with an S3 bucket for storing and retrieving data. To achieve this, the team must create a private S3 bucket, set appropriate IAM policies and roles, and test the application functionality.
Task:

1) EC2 Instance Setup:

    - An instance named `devops-ec2` already exists.
    - The instance requires access to an `S3 bucket`.

2) Setup SSH Keys:

    - Create new SSH key pair (`id_rsa` and `id_rsa.pub`) on the `aws-client` host and add the `public` key to the `root` user's `authorized keys` on the EC2 instance.

3) Create a Private S3 Bucket:

    - Name the bucket `devops-s3-18325`.
    - Ensure the bucket is `private`.

4) Create an IAM Policy and Role:

    - Create an IAM policy allowing `s3:PutObject`, `s3:ListBucket` and `s3:GetObject` access to `devops-s3-18325`.
    - Create an IAM role named `devops-role`.
    - Attach the policy to the IAM role.
    - Attach this role to the `devops-ec2` instance.

5) Test the Access:

    - SSH into the EC2 instance and try to upload a file to `devops-s3-18325` bucket using following command:

      ```sh
      aws s3 cp <your-file> s3://devops-s3-18325/
      ```
    - Now run following command to list the upload file:

      ```sh
      aws s3 ls s3://devops-s3-18325/
      ```

## Setup SSH Keys:

1. Create new SSH key pair (`id_rsa` and `id_rsa.pub`) on the `aws-client` host

   ```sh
   ssh-ketgen -t rsa
   ```

2. Allow `SSH` inbound rule for `0.0.0.0/0` in the `default` security group available.

    <img width="1335" height="261" alt="image" src="https://github.com/user-attachments/assets/85675b82-4287-4dd4-bf77-ef0bedbb22c4" />

3. Connect to the `devops-ec2` using `EC2 instance connect` from AWS console.

   <img width="1309" height="420" alt="image" src="https://github.com/user-attachments/assets/4d9d7228-a948-4811-bfdb-ca7453fdab4a" />

4. Add the `aws-clinet` host `public` key to the `root` user's `authorized_keys` on the EC2 instance.

   - Go to root directory

     ```sh
     sudo su -
     ```

   - Copy `public` key to `.ssh/authorized_keys`
  
  4. Access using `SSH` from `aws-client` to `devops-ec2`.

     ```sh
     ssh root@<devops-ec2-public-ip>
     ```


##  Create a Private S3 Bucket

1. Go to S3 bucket Dashborad and click `Create bucket`.

    <img width="312" height="188" alt="image" src="https://github.com/user-attachments/assets/a1bf6cdd-cb1c-48e1-beaa-1d360efa42fa" />

2. Enter bucket name under `General configuration` pane.

   <img width="1303" height="375" alt="image" src="https://github.com/user-attachments/assets/228ec553-e0cf-4fc1-b7ed-600ffb0fecaa" />

3. For object ownership, select `ACL disabled` in the `Object ownership` pane.

   <img width="1303" height="222" alt="image" src="https://github.com/user-attachments/assets/90ebf695-ce0b-4430-bf06-b0208bebfe82" />

4. To make sure bucket is private, make sure `Block all public access` is checked.

   <img width="1303" height="334" alt="image" src="https://github.com/user-attachments/assets/7e8e9cbf-6d1e-4198-8f53-92af714adc3d" />

5. Next, leave other settings to default and click `Create bucket`

6. Review bucket is created under `Amazon S3 > Buckets`

   <img width="743" height="315" alt="image" src="https://github.com/user-attachments/assets/966695e2-0c9c-40b1-814c-1d9f1acb3acf" />

## Create an IAM Policy

1. Go to IAM dashboard, and select `Policy` from navigation pane.

   <img width="233" height="422" alt="image" src="https://github.com/user-attachments/assets/2830e4bc-efcd-46c2-bcc7-f15278d503ac" />

2. Under `Policy editor`, choose `JSON` and add the following:

   <img width="1015" height="52" alt="image" src="https://github.com/user-attachments/assets/7f4be023-fd63-42b0-ab07-f3f332b2e06a" />

   ```json
   {
     "Version": "2012-10-17",
     "Statement": [
    {
      "Sid": "AllowListBucket",
      "Effect": "Allow",
      "Action": "s3:ListBucket",
      "Resource": "arn:aws:s3:::devops-s3-4378"
    },
    {
      "Sid": "AllowObjectOperations",
      "Effect": "Allow",
      "Action": [
        "s3:PutObject",
        "s3:GetObject"
      ],
      "Resource": "arn:aws:s3:::devops-s3-4378/*"
    }
    ]
    }
   ```
   
3. On the `Review and create` page, enter policy name and click `Create policy`.

   <img width="1049" height="275" alt="image" src="https://github.com/user-attachments/assets/73255215-08e1-4b06-a817-e1bfee1fe622" />

## Create an IAM role named `devops-role`.

1.  Go to IAM dashboard, and select `Role` from navigation pane.
   
    <img width="263" height="450" alt="image" src="https://github.com/user-attachments/assets/bbe1a0b6-c85f-4432-8079-502b550e351d" />

2. On `Step 1`, select `AWS service` for `Trusted entity type`.

   <img width="1015" height="349" alt="image" src="https://github.com/user-attachments/assets/d58c69d1-8758-4d32-9fa3-5ac4dce39d80" />

3. On `Step 1`, select `EC2` for `Service or use case` under `Use case`.

   <img width="1015" height="235" alt="image" src="https://github.com/user-attachments/assets/d2e95951-726b-4965-b500-e847fce93068" />

4. On `Step 2`, select `devops-policy` created earlier from search box.

   <img width="1015" height="223" alt="image" src="https://github.com/user-attachments/assets/910be93b-697b-46d7-ac0c-0b1a0e6fc3ad" />

5. On `Step 3`, enter role name and description(optional).

   <img width="1015" height="314" alt="image" src="https://github.com/user-attachments/assets/ce1801c4-0177-49ef-a0ec-a0084044d731" />

6. Click on `Create role`
   

## Attach role to EC2 instance

1. Go to EC2 instance dashboard and select `devops-ec2` instance.
2. Then select `Action > Security > Modify IAM Role` and select `devops-role`.
   
   <img width="1366" height="320" alt="image" src="https://github.com/user-attachments/assets/6aab4545-7b20-427f-87c2-02952284f446" />

##  Test the Access:

1.  SSH into the EC2 instance and try to upload a file to `devops-s3-18325` bucket using following command:

    - Create a text file `secret.txt`
      ```sh
      touch secret.txt
      ```
      Add some text (optional)

      ```sh
      aws s3 cp secret.txt s3://devops-s3-18325/
      ```
    Now run following command to list the upload file:

      ```sh
      aws s3 ls s3://devops-s3-18325/
      ```
