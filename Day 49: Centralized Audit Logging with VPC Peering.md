# Day 49: Centralized Audit Logging with VPC Peering

The Nautilus DevOps team needs to build a secure and scalable log aggregation setup within their AWS environment. The goal is to gather log files from an internal EC2 instance running in a private VPC, transfer them securely to another EC2 instance in a public VPC, and then push those logs to a secure S3 bucket.

A VPC named `datacenter-priv-vpc` already exists with a private subnet named `datacenter-priv-subnet`, a route table named `datacenter-priv-rt`, and an EC2 instance named `datacenter-priv-ec2` (using ubuntu image). This instance uses the SSH key pair `datacenter-key.pem` already available on the AWS client host at `/root/.ssh/`.

Your task is to:

- **Create a new VPC** named `datacenter-pub-vpc`.
- **Create a subnet** named `datacenter-pub-subnet` and a route table named `datacenter-pub-rt` under this public VPC.
- **Attach an internet gateway** to `datacenter-pub-vpc` and configure the public route table to enable internet access.
- **Launch an EC2 instance** named `datacenter-pub-ec2` into the public subnet using the same key pair as the private instance.
- **Create an IAM role** named `datacenter-s3-role` with `PutObject` permission to an S3 bucket and attach it to the public EC2 instance.
- **Create a new private S3 bucket** named `datacenter-s3-logs-18009`.
- **Configure a VPC Peering** named `datacenter-vpc-peering` between the private and public VPCs.
- Modify both `datacenter-priv-rt` and `datacenter-pub-rt` to route each other's CIDR blocks through the peering connection.
- On the private instance, **configure a cron job** to push the `/var/log/boots.log` file to the public instance (using `scp` or `rsync`).
- On the public instance, **configure a cron job** to push that same file to the created S3 bucket.
- The uploaded file must be stored in the S3 bucket under the path `datacenter-priv-vpc/boot/boots.log`.



## Task 1: Create a new VPC named `datacenter-pub-vpc`.

1. Go to VPC dashboard and click `Create VPC`.

   <img width="1313" height="459" alt="image" src="https://github.com/user-attachments/assets/417e5832-5559-4e60-978c-c6aada7778a8" />

2. Preview VPC after creation.

   <img width="1123" height="354" alt="image" src="https://github.com/user-attachments/assets/5c13e3a3-fce3-4815-b0e1-a8e80a1a8416" />


## Task 2: Create a subnet named `datacenter-pub-subnet`.

1. From VPC dashboard, select `Subnets` and click `Create subnet`.

   <img width="1318" height="223" alt="image" src="https://github.com/user-attachments/assets/c7c23321-5ec8-4f2c-93d9-1f085f3262af" />

2. Under `Subnet setings` section, enter subnet name and CIDR.

   <img width="1318" height="437" alt="image" src="https://github.com/user-attachments/assets/5dd6969a-73e6-4231-9641-320c8d94ea6b" />

3. Preview subnet after creating and you will see IPv4 not enabled.

   <img width="1101" height="411" alt="image" src="https://github.com/user-attachments/assets/a432f4c6-9716-484b-a741-e3d3d578bb5c" />

4. Click on `Action` > `Edit subnet settings` and enable auto assign public IPv4.

   <img width="1348" height="320" alt="image" src="https://github.com/user-attachments/assets/c0b309b3-5854-4da8-9b42-ad742879225d" />


## Task 3: Create an Internet Gateway

1. From VPC navigation pane, select internet gateway and click `Create internet gateway`.

   <img width="1365" height="470" alt="image" src="https://github.com/user-attachments/assets/9edf6e0f-c0dd-4b86-9626-205bfcd7ae3d" />

2. After internet gateway creation.

   <img width="1149" height="316" alt="image" src="https://github.com/user-attachments/assets/c444d478-2376-4582-adb0-a225ce9ee2ee" />

3. Attach internet gateway to `datacenter-pub-vpc`.

   <img width="1365" height="295" alt="image" src="https://github.com/user-attachments/assets/c0879d60-5305-43b1-a82c-3d2281df20f3" />

4. Preview after attaching.

   <img width="1149" height="304" alt="image" src="https://github.com/user-attachments/assets/1669c667-93da-4165-8878-42dfce3e6f0e" />


## Task 4: Create a route table named `datacenter-pub-rt`

1. From VPC navigation pane, select `Route table` and click `Create route table`.

   <img width="1366" height="484" alt="image" src="https://github.com/user-attachments/assets/9a582e2f-5522-48c5-b10e-a61d94b76942" />

2. Preview route table after creation.
  
   <img width="1173" height="382" alt="image" src="https://github.com/user-attachments/assets/f251f2d3-beb9-44c9-acfe-d5114aedf180" />

3. Attach route table to internet gateway.

   <img width="1366" height="360" alt="image" src="https://github.com/user-attachments/assets/b5de1a2c-8778-4797-9ecd-4513b084f8ac" />

4. Associate route table with the public subnet in public vpc.

   <img width="1105" height="208" alt="image" src="https://github.com/user-attachments/assets/d95e4e8e-9653-4d98-afea-6f83aa3318af" />

5. Verify subnet is public from public VPC resource map.

   <img width="1045" height="185" alt="image" src="https://github.com/user-attachments/assets/f5ba5797-a37e-4755-aa0b-413693be7350" />

## Task 5: Launch Public EC2 Instance

1. Enter instance name.

   <img width="862" height="117" alt="image" src="https://github.com/user-attachments/assets/1aff4280-803b-4e33-8ac1-b36d39d8efa1" />

2. Select AMI as Ubuntu.

   <img width="856" height="392" alt="image" src="https://github.com/user-attachments/assets/538665eb-3ea0-4f73-942b-ad447fe72cee" />

3. Instance type `t3.micro`.

   <img width="859" height="247" alt="image" src="https://github.com/user-attachments/assets/3bf8c587-2483-413f-bc3d-eaf806a2475c" />

4. For key pair, choose existing key pair `datacenter-keypair`.

   <img width="859" height="188" alt="image" src="https://github.com/user-attachments/assets/46faebe0-93ac-48c2-abdf-a76d16a114cb" />

5. Under `Networking`, select `datacenter-pub-vpc` and `datacenter-pub-subnet` and new security group `datacenter-pub-ec2-sg`.

   <img width="862" height="461" alt="image" src="https://github.com/user-attachments/assets/2b275d1f-11b5-4965-98f5-71db0ae181dc" />

6. Adjust security group for SSH.

   `SSH (22) >> datacenter-priv-vpc CIDR`

## Task 6: Create an IAM role named `datacenter-s3-role`

1. From IAM dashboard, select `Roles`.

   <img width="1015" height="350" alt="image" src="https://github.com/user-attachments/assets/c518f550-f8e2-47d3-bb8a-a119a953ee67" />

   <img width="1015" height="281" alt="image" src="https://github.com/user-attachments/assets/4c27c967-a857-40ff-81d5-7ee6ab0e8b25" />

2. Enter role name.

   <img width="1015" height="312" alt="image" src="https://github.com/user-attachments/assets/a0569dd0-6d98-4ed1-9e75-0c249051d803" />

3. Create the role.

4. Go to IAM → Policies → Create policy
   
6. Choose `JSON` and Paste this policy:

    ```json
    {
      "Version": "2012-10-17",
      "Statement": [
        {
          "Effect": "Allow",
          "Action": "s3:PutObject",
          "Resource": "arn:aws:s3:::datacenter-s3-logs-18009/*"
        }
      ]
    }
    ```

   <img width="1334" height="443" alt="image" src="https://github.com/user-attachments/assets/b7ebb9ce-3faa-49b8-b113-3dc3a050b460" />

6. Enter polic name to select the policy.

   <img width="1015" height="306" alt="image" src="https://github.com/user-attachments/assets/6a6debe0-4e16-4bae-899a-4e617a669364" />

7. Attach Policy to Role, Open `datacenter-s3-role`, Click `Add permissions`.

   <img width="1366" height="495" alt="image" src="https://github.com/user-attachments/assets/55d42c0b-4a0b-4c12-9d60-f8771c9746c1" />

8. Attach role to `datacenter-pub-ec2`, from `Action` > `Security` > `Modify IAM role` and select `datacenter-s3-role`.


## Task 7: Create S3 Bucket

1. Enter bucket name.

   <img width="1299" height="434" alt="image" src="https://github.com/user-attachments/assets/30ac2ef4-3f47-4eb7-a9aa-d3709fa93ce0" />

2. To make the bucket private, check `Block all public access`.

   <img width="1299" height="397" alt="image" src="https://github.com/user-attachments/assets/e38ca60b-1810-44ba-8f93-814238cf2eff" />

3. Enable encryption as follows:

   <img width="1299" height="308" alt="image" src="https://github.com/user-attachments/assets/b38e91a0-e4d8-452d-8d63-313f3b50cacd" />

4. Create the bucket.

   <img width="859" height="237" alt="image" src="https://github.com/user-attachments/assets/6c5d85c7-2e16-4e73-a4ba-84f2d1c42ab3" />


## Task 8: VPC peering

1. From VPC navigation manu, select `Peering connection` and Enter VPC peering name.

   <img width="1315" height="130" alt="image" src="https://github.com/user-attachments/assets/2ad8baa9-840d-4987-af2f-79b868b5b1a0" />

2. Select requester `datacenter-priv-vpc` and Accepter `datacenter-pub-vpc`.

   <img width="1294" height="472" alt="image" src="https://github.com/user-attachments/assets/14c8cdb7-bf59-4349-99f7-4e31222a66af" />

3. Accept request for vpc.

   <img width="820" height="319" alt="image" src="https://github.com/user-attachments/assets/7a57301c-6278-45cf-a3fb-944279529a58" />


## Task 9:  Update route tables for both VPC

1. For `datacenter-priv-rt`, point `datacenter-pub-vpc` CIDR to internet gateway.

   <img width="1108" height="450" alt="image" src="https://github.com/user-attachments/assets/b4c4696b-7be4-46f3-977f-2d4de5de526b" />

2. For `datacenter-pub-rt`, point `datacenter-priv-vpc` CIDR to internet gateway.

   <img width="1108" height="452" alt="image" src="https://github.com/user-attachments/assets/342d8f96-5702-443d-a435-27686f289af5" />


## Task 10: Setup cronjobs in each instance

1. SSH into `datacenter-pub-ec2` from `aws-client` host using it's public ip.

   ```sh
   ssh -i .ssh/datacenter-key.pem ubuntu@<datacenter-pub-ec2-public-ip>
   ```
   
2. SSH into private instance from `aws-client` host and enable SSH access to public instance.

   ```sh
   ssh ubuntu@<datacenter-priv-ec2-priv-ip> -J ubuntu@<datacenter-pub-ec2-pub-ip>
   ```

   **Now create RSA keys in `datacenter-priv-ec2`.**

   ```sh
   ssh-keygen -t rsa
   ```

   **Copy the public key `id_rsa.pub` to `datacenter-pub-ec2`'s `authorized_keys` and try ssh into `datacenter-pub-ec2` using it's private ip from private instance.**

   **Set cronjob in `datacenter-priv-ec2`.**

   ```sh
   * * * * * scp /var/log/boots.log ubuntu@10.20.1.53:~/boot/boots.log
   ```

3. **Set cronjob in `datacenter-pub-ec2`.**

   **Install aws cli first:**

   ```sh
   sudo apt install unzip
   ```
   
   ```sh
   curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
   ```

   ```sh
   sudo unzip awscliv2.zip
   sudo ./aws/install
   ```

   **Create new access key for `kk_lab_user_xxxx` in IAM:**

   <img width="1018" height="211" alt="image" src="https://github.com/user-attachments/assets/878dccd1-799d-4a43-a32f-dcab4e681635" />
   
   **Set aws credential:**
   
   ```sh
   aws configure
   ```

   - **Enter the following details**:

     - **AWS Access Key ID**: Enter the Access Key ID from the previous step.
     - **AWS Secret Access Key**: Enter the Secret Access Key from the previous step.
     - **Default region name**: Enter the AWS region you want to use (e.g., us-west-1).
     - **Default output format**: Enter the output format (e.g., json, text, or table).


   **Set cronjob in `datacenter-pub-ec2`.**

   ```sh
   * * * * * aws s3 cp ~/boot/boots.log s3://datacenter-s3-logs-20551/datacenter-priv-vpc/boot/boots.log
   ```
   
   **Wait for some minutes and check S3 bucket:**
   
   <img width="1018" height="297" alt="image" src="https://github.com/user-attachments/assets/451a2d89-46e4-43f1-8b9e-228b58799202" />
