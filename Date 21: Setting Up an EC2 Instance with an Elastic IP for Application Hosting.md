# Date 21: Setting Up an EC2 Instance with an Elastic IP for Application Hosting

The Nautilus DevOps Team has received a new request from the Development Team to set up a new EC2 instance. This instance will be used to host a new application that requires a stable IP address. To ensure that the instance has a consistent public IP, an Elastic IP address needs to be associated with it. The instance will be named `datacenter-ec2`, and the `Elastic IP` will be named `datacenter-eip`. This setup will help the Development Team to have a reliable and consistent access point for their application.

Create an EC2 instance named `datacenter-ec2` using any linux AMI like ubuntu, the Instance type must be `t2.micro` and associate an `Elastic IP` address with this instance, name it as `datacenter-eip`.


[Step 1: Launch an instance](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EC2_GetStarted.html#ec2-launch-instance)



1. In the navigation bar at the top of the screen, we display the current AWS Region — for example, Ohio. You can use the selected Region, or optionally select a Region that is closer to you.

2. From the EC2 console dashboard, in the `Launch instance` pane, choose `Launch instance`.

3. Under `Name and tags`, for Name, enter a descriptive name for your instance.

   <img width="859" height="131" alt="image" src="https://github.com/user-attachments/assets/8241dcc9-9ce8-4535-908f-ad8b58c281b5" />

5. Under Instance type, for Instance type, select an instance type that is marked Free Tier eligible.

   <img width="859" height="247" alt="image" src="https://github.com/user-attachments/assets/5226179d-ebf0-40c8-953c-5601788e89e4" />

4. Under `Key pair` (login), for Key pair name, choose an existing key pair or choose Create new key pair to create your first key pair.
5. Review a summary of your instance configuration in the `Summary` panel, and when you're ready, choose `Launch instance`.

   <img width="414" height="472" alt="image" src="https://github.com/user-attachments/assets/087d9abb-ba80-4c4e-91ee-958b2e284473" />

[Step 2: Allocate an Elastic IP address](https://docs.aws.amazon.com/vpc/latest/userguide/WorkWithEIPs.html#allocate-eip)

1. Open the Amazon VPC console at [https://console.aws.amazon.com/vpc/](https://console.aws.amazon.com/vpc/)

   <img width="1063" height="477" alt="image" src="https://github.com/user-attachments/assets/98754727-0c03-4089-8366-d0ef70276e07" />

3. In the navigation pane, choose `Elastic IPs`.

   <img width="223" height="488" alt="image" src="https://github.com/user-attachments/assets/d30caad4-fb88-4546-baa5-0faf84d0841a" />

5. Choose `Allocate Elastic IP address`.

   <img width="1125" height="184" alt="image" src="https://github.com/user-attachments/assets/f8f02fc0-0a65-4beb-b2c8-f92d66db092f" />
   
6. Rename Elastic IP with `datacenter-eip`
   
   <img width="1125" height="203" alt="image" src="https://github.com/user-attachments/assets/37a3d0dd-bc83-4f21-89fd-7640102c79e2" />

[Step 3: Associate an Elastic IP address](https://docs.aws.amazon.com/vpc/latest/userguide/WorkWithEIPs.html#associate-eip)

- Open the Amazon VPC console at [https://console.aws.amazon.com/vpc/](https://console.aws.amazon.com/vpc/)
- In the navigation pane, choose `Elastic IPs`.

  <img width="223" height="474" alt="image" src="https://github.com/user-attachments/assets/e6fd0ec7-6f54-45e3-814c-c0264125347b" />

- Select an Elastic IP address that's allocated for use with a VPC (the Scope column has a value of vpc), and then choose `Actions, Associate Elastic IP address`.

  <img width="1125" height="337" alt="image" src="https://github.com/user-attachments/assets/7a009371-71bf-4540-99b6-ef8c90abd337" />

- Choose `Instance` or `Network interface`, and then select either the instance or network interface ID. Select the private IP address with which to associate the Elastic IP address. Choose `Associate`.

  <img width="1348" height="505" alt="image" src="https://github.com/user-attachments/assets/0d59779c-1807-47d0-aa7c-5fe05d1ba96b" />

