# Day 44: Implementing Auto Scaling for High Availability in AWS

The DevOps team is tasked with setting up a highly available web application using AWS. To achieve this, they plan to use an Auto Scaling Group (ASG) to ensure that the required number of EC2 instances are always running, and an Application Load Balancer (ALB) to distribute traffic across these instances. The goal of this task is to set up an ASG that automatically scales EC2 instances based on CPU utilization, and an ALB that directs incoming traffic to the instances. The EC2 instances should have Nginx installed and running to serve web traffic.

- Create an EC2 launch template named `xfusion-launch-template` that specifies the configuration for the EC2 instances, including the `Amazon Linux 2 AMI`, `t2.micro` instance type, and a security group that allows HTTP traffic on port 80.
- Add a `User Data` script to the launch template to install `Nginx` on the EC2 instances when they are launched. The script should install Nginx, start the Nginx service, and enable it to start on boot.
- Create an Auto Scaling Group named xfusion-asg that uses the launch template and ensures a minimum of `1` instance, desired capacity is `1` instance and a maximum of `2` instances are running based on CPU utilization. Set the target CPU utilization to `50%`.
- Create a target group named `xfusion-tg`, an Application Load Balancer named `xfusion-alb` and configure it to listen on port `80`. Ensure the ALB is associated with the Auto Scaling Group and distributes traffic across the instances.
- Configure health checks on the ALB to ensure it routes traffic only to healthy instances.
- Verify that the ALB's DNS name is accessible and that it displays the default Nginx page served by the EC2 instances.


>[!Note]
> Create the resources only in `us-east-1` region.

## Task 1: Create security groups
1. Go to EC2 dashboard and select `Security groups` from navigation menu.

   <img width="197" height="459" alt="image" src="https://github.com/user-attachments/assets/88f612c1-770b-45c3-95d0-9184b7b8f0fc" />
   
Create two security groups under default VPC. 

- One for **ALB**
  - Name: `xfusion-alb-sg`
  - Description: Allow public HTTP access
  - **Inbound rules:**
    - Type: **HTTP**
    - Port: **80**
    - Source: **Anywhere (0.0.0.0/0)**
  - **Outbound:** Allow all

<img width="1149" height="422" alt="image" src="https://github.com/user-attachments/assets/5ab39784-e4d5-4e0f-8ef9-c87aaff79df2" />

- One for **EC2 instances**
  - Name : `xfusion-ec2-sg`
  - Description: Allow HTTP from ALB
  - **Inbound rules:**
    - Type: **HTTP**
    - Port: **80**
    - Source: **xfusion-alb-sg**
  - **Outbound:** Allow all

<img width="1149" height="428" alt="image" src="https://github.com/user-attachments/assets/5d19b793-a8b0-4fcd-aa15-b9e80706cc41" />

## Task 2: Create an EC2 launch template

1. Go to EC2 dashboard and select `Launch template` from navigation menu.

   <img width="223" height="435" alt="image" src="https://github.com/user-attachments/assets/a8d5c315-c0b7-48cb-b3bf-d53b54a242c6" />

2. Click on `Create launch template` in `New launch template` box.

   <img width="346" height="120" alt="image" src="https://github.com/user-attachments/assets/7e8c99a8-33b8-42e3-842b-83beb032450f" />

3. Enter launch template name and description(optional)

   <img width="862" height="366" alt="image" src="https://github.com/user-attachments/assets/55532aac-5fc0-4b69-9786-bd617675bf31" />

4. For AMI, select `Amazon Linux 2023 (kernel-6.1)`.

   <img width="862" height="452" alt="image" src="https://github.com/user-attachments/assets/fd47e85a-2dcc-4fba-adeb-ef0665d6258e" />

5. For `Instace type`, select `t2.micro`.

   <img width="862" height="212" alt="image" src="https://github.com/user-attachments/assets/20cfeb3a-194c-4f17-a5f1-fbe21005dd51" />

6. Under `Network settings`, select only `xfusion-ec2-sg` security group and leave rest as it is.

   <img width="862" height="427" alt="image" src="https://github.com/user-attachments/assets/1bc4fe4f-eba0-4074-9e6f-cbe764d242bf" />

7. Next in the `Advance details` section, add following script in `User data` section.

   ```sh
   #!/bin/bash
   dnf update -y
   dnf install nginx -y
   systemctl start nginx
   systemctl enable nginx
   ```

8. Click `Create launch template`.

   <img width="417" height="346" alt="image" src="https://github.com/user-attachments/assets/e859884a-a3fc-4c64-a0b6-33513a92a9f6" />



## Task 3: Create target group


1. Go to EC2 dashboard and select `Target groups` from navigation menu,

   <img width="223" height="414" alt="image" src="https://github.com/user-attachments/assets/1bd2a7ef-40f4-4590-bb3b-a72cda9d314c" />

2. Click on `Create target group`.
3. Under `Settings`, section, select `Target type` as `Instance`.

   <img width="1015" height="390" alt="image" src="https://github.com/user-attachments/assets/baeeb953-1adc-4889-9917-8081348959db" />

4. Enter target group name and define ports.

   <img width="1050" height="214" alt="image" src="https://github.com/user-attachments/assets/fbfe0c15-e2d8-44eb-8ab3-142a6062c00b" />

5. Also define VPC as `default` VPC and IP Address type as `IPv4`.

   <img width="1015" height="466" alt="image" src="https://github.com/user-attachments/assets/0742b869-6374-4200-ab83-4b9bf7720b18" />

6. Click `Create target group`.


## Task 4: Create Application Load Balancer

1. Go to EC2 dashboard and select `Load balancers` from navigation menu,

   <img width="223" height="421" alt="image" src="https://github.com/user-attachments/assets/9ebcc54d-3727-47e2-a63c-136fe72d7afd" />

2. Unde `Basic configuration`, enter name and select `Scheme` as `Internet facing`.

   <img width="1294" height="416" alt="image" src="https://github.com/user-attachments/assets/1d0d8b17-1b3e-487c-94aa-87df39adf039" />

3. Under `Network mapping`, select `default` VPC and two subnet.

   <img width="1294" height="473" alt="image" src="https://github.com/user-attachments/assets/b7d79980-07ee-4cce-81b0-994ad7090b60" />

4. Select `xfusion-alb-sg` secutity group for the ALB.

   <img width="1294" height="166" alt="image" src="https://github.com/user-attachments/assets/87f550f3-b5e9-4ac2-80a9-812593cb1d2b" />

5. Next under `Listeners and routing`, select previously created target group.

   <img width="1260" height="336" alt="image" src="https://github.com/user-attachments/assets/9d40e243-3baf-440e-82f6-e287407b7879" />

6. CLick `Create load balancer`.
7. Wait for the load balancer `Status` to be appear as `Active`.

   <img width="1088" height="295" alt="image" src="https://github.com/user-attachments/assets/cfd7f231-686b-4ab8-8d35-f6259523bc2c" />


## Task 5: Create an Auto Scaling Group

1. Go to EC2 dashboard and select `Auto Scaling group` from navigation menu,

   <img width="223" height="405" alt="image" src="https://github.com/user-attachments/assets/bf19ac4c-bfb2-4402-a66e-f30faba9502a" />

2. Click on `Create Auto Scaling group` in `Create Auto Scaling group` box.

   <img width="332" height="191" alt="image" src="https://github.com/user-attachments/assets/640cee07-c684-491d-8877-325c470b935f" />

3. Enter auto scalling group name.

   <img width="975" height="164" alt="image" src="https://github.com/user-attachments/assets/9ddf57e4-e6ae-4f26-8472-113a9805b205" />

4. Select the launch template created earlier and click `Next`.

   <img width="1013" height="410" alt="image" src="https://github.com/user-attachments/assets/e1b29e56-aca0-48fb-983a-56c9669256c9" />

5. Under `Network`, select default VPC and any subnet while leave others as they are and click `Next`.

   <img width="1013" height="411" alt="image" src="https://github.com/user-attachments/assets/ac62002f-e613-4c9d-a466-3219062f7e3c" />

>[!Note]
> **Select same subnets used by ALB**

6. In the `Load balancing` section, Select `Attach to an existing load balancer` and attach previously created target group.

   <img width="1013" height="428" alt="image" src="https://github.com/user-attachments/assets/cf30b1d9-cb72-47a5-b57f-410b9e1e7240" />

6. Enable health check by clicking `Turn on Elastic Load Balancer health check`.

   <img width="1013" height="443" src="https://github.com/user-attachments/assets/aecdc3e2-165f-4098-924b-3b69b0573a87" />

8. Ensures a minimum of `1` instance, desired capacity is `1` instance and a maximum of `2` instances are running based on CPU utilization. Set the target CPU utilization to `50%`.

   <img width="1013" height="230" alt="image" src="https://github.com/user-attachments/assets/8ad60356-d471-4ccb-91cf-6a581dcd9c34" />
   <img width="1013" height="187" alt="image" src="https://github.com/user-attachments/assets/75136a7d-cefd-4097-85f5-052263789f58" />
   <img width="1013" height="474" alt="image" src="https://github.com/user-attachments/assets/1afa0472-10fe-497f-9eeb-cf8dd8e9a4cc" />
   <img width="1013" height="274" alt="image" src="https://github.com/user-attachments/assets/b3915a35-5b74-47e1-b596-b2098645b92d" />

9. Leave rest of the settings as default and click `Create auto scalling group`.


## Task 6: Access application via web

1. Go to load balancer dashboard and select `xfusion-alb`.

   <img width="1132" height="368" alt="image" src="https://github.com/user-attachments/assets/d6641c27-2d8a-4902-9e19-96838af36a9d" />

2. Enter `xfusion-alb` DNS name to a browser and you should see nginx default page.

   <img width="1366" height="303" alt="image" src="https://github.com/user-attachments/assets/964d8609-7683-419e-8a99-5b379817f474" />


   

   
   
