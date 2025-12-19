# Day 26: Configuring an EC2 Instance as a Web Server with Nginx

As a member of the Nautilus DevOps Team, your task is to create an EC2 instance with the following specifications:

**Instance Name**: The EC2 instance must be named `datacenter-ec2`.

**AMI**: Use any available `Ubuntu` AMI to create this instance.

**User Data Script**: Configure the instance to run a user data script during its launch. This script should:
- `Install` the Nginx package.
- `Start` the Nginx service.
  
**Security Group**: Ensure that the instance allows `HTTP` traffic on port `80` from the internet.


### Create an Amazon EC2 instance

Log in to the EC2 dashboard in the AWS Management Console: [https://console.aws.amazon.com/ec2.](https://console.aws.amazon.com/ec2.)

- In the left navigation bar, choose **Instances**.
- Then select **Launch Instances** in the top right corner.
- On the Launch an Instance page, give your new instance a name in the **Name** and **Tags** section. This name will show in the Name column of the summary table on the EC2 Instances dashboard. This guide is using "datacenter-ec2"

  <img width="859" height="130" alt="image" src="https://github.com/user-attachments/assets/ae7dc332-df73-4254-b12e-033844e0be79" />

- In the **Application and OS Images** (Amazon Machine Image) section select the image of the Linux distribution of `Ubuntu`.

  <img width="848" height="461" alt="image" src="https://github.com/user-attachments/assets/c5a579c9-898e-46fd-a4fd-c141c42ce43e" />

- In the **Key pair (login)** section, select an existing key pair or create a new one. If you choose **Create new key pair**, a window appears, allowing you to download the key pair.

  <img width="859" height="190" alt="image" src="https://github.com/user-attachments/assets/b0c28ecf-b38f-4c72-a6cb-f95d2fa3e023" />

- Scroll to the **Network settings** section. You can keep the default **VPC and Subnet settings**. Under Firewall (Security Groups), either create a new security group or use an existing one.
  - Select `Allow HTTP` traffic from the `internet`.
  - Select `Allow SSH` traffic from the `internet`.
    
  <img width="855" height="272" alt="image" src="https://github.com/user-attachments/assets/dc312feb-100f-453d-877b-653bd1a78c44" />

- Under **Advance details**, Add following user script for Nginx installation ans start.

  ```bash
  #!/bin/bash
  apt update -y
  apt install -y nginx
  systemctl start nginx
  systemctl enable nginx
  ```

- Now click `Lanch instance` and connect to the instance using the keypair and its public ip address.

  - User Name: `ubuntu`

- Verify Nginx service.
  - `systemctl status nginx`

    <img width="762" height="246" alt="image" src="https://github.com/user-attachments/assets/bc8b7d9a-7fcf-4212-9905-cd227e3bf679" />

  - Visit instance's public dns with a browser.

    <img width="818" height="331" alt="image" src="https://github.com/user-attachments/assets/5577eeaf-ce0d-4567-9685-4e3efa90d86c" />

    

  
