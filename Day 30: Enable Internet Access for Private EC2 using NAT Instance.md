# Day 30: Enable Internet Access for Private EC2 using NAT Instance

The Nautilus DevOps team is tasked with enabling internet access for an EC2 instance running in a private subnet. This instance should be able to upload a test file to a public S3 bucket once it can access the internet. To minimize costs, the team has decided to use a NAT Instance instead of a NAT Gateway.

The following components already exist in the environment:
1) A VPC named `devops-priv-vpc` and a private subnet named `devops-priv-subnet` have been created.
2) An EC2 instance named `devops-priv-ec2` is already running in the **private subnet**.
3) The EC2 instance is configured with a cron job that uploads a test file to the S3 bucket `devops-nat-18335` every minute. Upload will only succeed once internet access is established.

Your task is to:

- Create a new public subnet named `devops-pub-subnet` in the existing VPC.
- Launch a **NAT Instance** in the public subnet using an `Amazon Linux 2` AMI and name it `devops-nat-instance`. Configure this instance to act as a **NAT instance**. Make sure to use a custom security group for this instance.

After the configuration, verify that the test file `devops-test.txt` appears in the S3 bucket `devops-nat-18335`. This indicates successful internet access from the private EC2 instance via the NAT Instance.

**AWS Docs to be followed**: [Create a NAT AMI](https://docs.aws.amazon.com/vpc/latest/userguide/work-with-nat-instances.html#create-nat-ami)

## Create a new public subnet named `devops-pub-subnet` in the existing VPC `devops-priv-vpc`

1. Open the Amazon VPC console at [https://console.aws.amazon.com/vpc/](https://console.aws.amazon.com/vpc/)

2. In the navigation pane, choose `Create subnet` and select VPC ID of `devops-priv-vpc`.

   <img width="1315" height="249" alt="image" src="https://github.com/user-attachments/assets/db913293-9e5f-436f-8873-7ec3eda7676b" />

4. Enter name `devops-pub-subnet` and ensure availabe non-overlapping CIDR for subnet.

   <img width="1275" height="376" alt="image" src="https://github.com/user-attachments/assets/7a9b8039-3faf-4ba8-afbe-7e6cf1f901cf" />

5. To make `devops-pub-subnet` a public, ensure this subnet is associated with a Route Table that has a path to the Internet Gateway (IGW).
   - Create new route table `public-route-table` under `devops-priv-vpc`.

     <img width="1315" height="207" alt="image" src="https://github.com/user-attachments/assets/d0313760-07c3-43b7-bcb4-d26c7a6ea903" />

   - Create a new internet gateway `igw-priv-vpc` and attach it to `devops-priv-vpc`.

     <img width="1347" height="299" alt="image" src="https://github.com/user-attachments/assets/767449d5-e753-4b46-9d34-cd0f430b3cac" />

   - The new route table `public-route-table` will now have attached to internet gateway.
  
     <img width="1074" height="175" alt="image" src="https://github.com/user-attachments/assets/aa773f44-2bc9-4f63-ad7a-986151e8c08d" />

   - Now associate the `devops-pub-subnet` to the `public-route-table`.

     <img width="1345" height="441" alt="image" src="https://github.com/user-attachments/assets/3abf7e79-185f-4ce0-9999-0de1286a3280" />

   - `devops-pub-subnet` will now act as a public subnet.

     <img width="938" height="196" alt="image" src="https://github.com/user-attachments/assets/345b6b3a-db89-44a8-b385-0bef26ba35b0" />

6. Now create `devops-nat-instance` under `devops-pub-subnet` using an `Amazon Linux 2` AMI and configure this instance to act as a **NAT instance**.

   <img width="851" height="354" alt="image" src="https://github.com/user-attachments/assets/f8166351-daec-4aac-b6d8-8a45a1ebb305" />

7. Make sure to choose `devops-priv-vpc` and subnet `devops-pub-subnet`. Also enable `Auto-assign public IP`

   <img width="859" height="426" alt="image" src="https://github.com/user-attachments/assets/f43bdb4c-438b-41e3-9198-3cd6b25706d3" />

8. Change **inbound** security group rules for instance as follows:
   - Choose **Add rule**. Choose **HTTP** for Type and enter the IP address range of **your private subnet** for **Source**.
   - Choose **Add rule**. Choose **HTTPS** for Type and enter the IP address range of **your private subnet** for **Source**.
   - Choose **Add rule**. Choose **SSH** for Type and enter the IP address range of **your network** for **Source**.

     <img width="1315" height="369" alt="image" src="https://github.com/user-attachments/assets/94b5d6bf-c7cd-475f-bc62-a28b2a59547e" />

10. Change **outbound** security group rules for instance as follows:

    - Choose **Add rule**. Choose **HTTP** for Type and enter **0.0.0.0/0** for **Destination**.
    - Choose **Add rule**. Choose **HTTPS** for Type and enter **0.0.0.0/0** for **Destination**.

      <img width="1315" height="370" alt="image" src="https://github.com/user-attachments/assets/5a5ee93f-28a3-43c9-a0a0-9c0af5f78a0d" />

12. **Disable Source/Dest Check**: (Critical) Select `Instance > Actions > Networking > Change source/destination check > Set to Stop/Disable`.

    <img width="600" height="343" alt="image" src="https://github.com/user-attachments/assets/32b9fc07-85ad-4976-ba0b-ca89664bb4db" />


## To create a NAT AMI for Amazon Linux

1. Configure OS-Level NAT (IP Tables)

   SSH into the NAT instance and run the following:

   ```bash
   sudo yum install iptables-services -y
   sudo systemctl enable iptables
   sudo systemctl start iptables
   ```
   Enable IP forwarding in the kernel
   ```bash
   echo "net.ipv4.ip_forward = 1" | sudo tee -a /etc/sysctl.conf
   sudo sysctl -p
   ```
   Configure IP tables to route traffic from the private subnet
   ```bash
   sudo iptables -t nat -A POSTROUTING -o eth0 -s 10.1.1.0/24 -j MASQUERADE
   sudo service iptables save
   ```

   <img width="1343" height="390" alt="image" src="https://github.com/user-attachments/assets/5bac1bf5-5113-4eeb-b4c2-8e96f69c0ed9" />
   <img width="1356" height="143" alt="image" src="https://github.com/user-attachments/assets/918d3f37-a9cc-4d32-a782-244ee4434776" />
   <img width="1364" height="153" alt="image" src="https://github.com/user-attachments/assets/48db3553-7cec-4634-8c00-0e29188dabac" />

2. Update Private Subnet Routing (The "Private" Side)

   Add Route: Destination: `0.0.0.0/0` -> Target: Instance -> Select your `devops-nat-instance`.
   
   <img width="1366" height="344" alt="image" src="https://github.com/user-attachments/assets/ddddf4a7-2378-4cff-ae8c-98ef61ae3ade" />
