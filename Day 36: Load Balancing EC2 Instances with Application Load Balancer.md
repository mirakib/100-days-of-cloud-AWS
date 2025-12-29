# Day 36: Load Balancing EC2 Instances with Application Load Balancer

The Nautilus Development Team needs to set up a new EC2 instance and configure it to run a web server. This EC2 instance should be part of an Application Load Balancer (ALB) setup to ensure high availability and better traffic management. The task involves creating an EC2 instance, setting up an ALB, configuring a target group, and ensuring the web server is accessible via the ALB DNS.

1. **Create a security group**: Create a security group named `datacenter-sg` to open port `80` for the `default` security group (which will be attached to the ALB). Attach `datacenter-sg` security group to the EC2 instance.

2. **Create an EC2 instance**: Create an EC2 instance named `datacenter-ec2`. Use any available `Ubuntu` AMI to create this instance. Configure the instance to run a `user data` script during its launch.

   This script should:
   - Install the `Nginx` package.
   - Start the `Nginx` service.

3. **Set up an Application Load Balancer**: Set up an Application Load Balancer named `datacenter-alb`. Attach default security group to the same.

4. **Create a target group**: Create a target group named `datacenter-tg`.

5. **Route traffic**: The ALB should route traffic on port `80` to port `80` of the `datacenter-ec2` instance.

6. **Security group adjustments**: Make appropriate changes in the default security group attached to the ALB if necessary. Eventually, the Nginx server running under `datacenter-ec2` instance must be accessible using the ALB DNS.


## Create security group

1. Go to EC2 dashboard and select `Security Groups` from navigation pane.

   <img width="197" height="466" alt="image" src="https://github.com/user-attachments/assets/61bc0d84-b2dc-4193-a5f4-80fb4cfb83e0" />

2. Create new security group `datacenter-sg` for `datacenter-ec2` to use.

   <img width="1318" height="243" alt="image" src="https://github.com/user-attachments/assets/c6c48ad2-4faf-41b2-a40b-90209d9780c2" />

3. Add inbound rule for port `80` to `default` security group already available.

   <img width="1318" height="177" alt="image" src="https://github.com/user-attachments/assets/3a2f7482-1488-4b0b-a312-9ca9894ea506" />

4. Now we should have two security group.

   <img width="1149" height="166" alt="image" src="https://github.com/user-attachments/assets/c33d9ff9-3798-4bb1-8618-05179303d2d9" />


## Adjust default security group 

1. Add inbound port rule `HTTP (80)` from `0.0.0.0/0`

   <img width="1149" height="447" alt="image" src="https://github.com/user-attachments/assets/895ddc0c-38d8-41c5-b75c-42c7d8d90d50" />

2. Click on `Save`

   
## Create EC2 instance

1. Go to EC2 Dashboard and click on `Launch Instance`

   <img width="349" height="221" alt="image" src="https://github.com/user-attachments/assets/cff4ad39-053a-4eee-9466-a27c184b822a" />

2. Enter name `datacenter-ec2`.

   <img width="862" height="115" alt="image" src="https://github.com/user-attachments/assets/f7aa9e7a-59ad-4953-9e27-4cba6a264a61" />

3. For AMI, select `Ubuntu`.

   <img width="862" height="276" alt="image" src="https://github.com/user-attachments/assets/98a72b19-df82-4676-b13d-788d77185f07" />

4. Next, under `Network settings` pane, select `Existing security group` and choose `datacenter-sg`.

    <img width="862" height="438" alt="image" src="https://github.com/user-attachments/assets/3167d5f9-da16-4fb9-b45e-65fe4a4aabc3" />

5. Next go to `Advance settings` and enter the following code in `User data`

   ```sh
   #!/bin/bash
   apt update -y
   apt install -y nginx
   systemctl start nginx
   systemctl enable nginx
   ```
   
5. Now click `Launch instance` and wait for the instance to get status `Running`


## Create targer group

1. Go to EC2 dashborad and select `Target Groups` from navigation pane.

   <img width="214" height="419" alt="image" src="https://github.com/user-attachments/assets/225ad143-8b76-4a56-a9e7-ab042d8640b1" />

2. Select `Target type` as `Instance` then enter name and define ports.

   <img width="1067" height="473" alt="image" src="https://github.com/user-attachments/assets/19a59704-46a8-4a0b-918f-7ea5a58d84fa" />

3. Click `Next` and select the `datacenter-ec2` instance and port.

   <img width="1015" height="445" alt="image" src="https://github.com/user-attachments/assets/bb56921e-45f7-46e9-95e6-caf3065d1963" />
   
   **Must click on `Include as pending below` button**
   After clicking `Include as pending below`, it should appear in the `Targets` section.
   
   <img width="1015" height="253" alt="image" src="https://github.com/user-attachments/assets/889b1621-41e4-4743-af07-0e59583652de" />

5. Next on the `Review and create` tab, Review all the steps and click on `Create target group`

   <img width="1015" height="183" alt="image" src="https://github.com/user-attachments/assets/0c5d624f-4922-4466-8049-2813f75521a7" />
   <img width="1015" height="185" alt="image" src="https://github.com/user-attachments/assets/a5fd5809-3623-46eb-a1fa-552577d8734d" />
   <img width="1015" height="133" alt="image" src="https://github.com/user-attachments/assets/ff17e396-a3da-4195-ace1-56ad7c732799" />

## Create Application Load Balancer

1. Go to EC2 dashborad and select `Target Groups` from navigation pane.

   <img width="214" height="419" alt="image" src="https://github.com/user-attachments/assets/225ad143-8b76-4a56-a9e7-ab042d8640b1" />

2. Click on the `Create Load Balancer` drop down and select `Application Load Balancer`.

   <img width="1102" height="251" alt="image" src="https://github.com/user-attachments/assets/eac5ccc0-8b8c-468e-af4b-f88ec17e4d96" />

3. Under `Basic configuration` pane, enter name and select `Scheme` as `Internet-facing` and `IPv4` load balancer IP address type.

   <img width="1303" height="467" alt="image" src="https://github.com/user-attachments/assets/ea518567-8e95-47d8-b9f3-6397319121e3" />

4. On the `Network mapping` pane, select `default` vpc and at least two subnets.

   <img width="1303" height="427" alt="image" src="https://github.com/user-attachments/assets/c9428a74-d727-4d08-9b6f-79bb6a41669c" />
   
   **`Make sure you select the subnet of your datacenter-ec2 instance is on`**
   
6. For `Security Group`, select `default`

   <img width="1303" height="186" alt="image" src="https://github.com/user-attachments/assets/330d50b8-2226-43c0-9375-95fd61dffdfa" />

7. On the `Listeners and routing` section, select `datacenter-tg` as target group.

   <img width="1266" height="433" alt="image" src="https://github.com/user-attachments/assets/7952a1c7-f5c9-4818-97b1-e53309e076ac" />

8. CLick on `create load balancer`.

## Verification

1. Check `datacenter-lb` is in `Active` status.

   <img width="1132" height="360" alt="image" src="https://github.com/user-attachments/assets/55908783-1853-4d2f-9c23-096e2e1f0ee3" />

2. Enter the DNS nama to a browser.

   <img width="528" height="73" alt="image" src="https://github.com/user-attachments/assets/1060f494-8a31-4194-8715-9022d1026ba4" />

3. You should see Nginx welcome page

   <img width="802" height="346" alt="image" src="https://github.com/user-attachments/assets/9ba26274-93b3-47da-8a13-4db6004e92f8" />
