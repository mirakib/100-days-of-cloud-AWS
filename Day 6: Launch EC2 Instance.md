# Day 6: Launch EC2 Instance

For this task, create an EC2 instance with following requirements:

1) The name of the instance must be `devops-ec2`.

2) You can use the Amazon Linux AMI to launch this instance.

3) The Instance type must be `t2.micro`.

4) Create a new RSA key pair named `devops-kp`.

5) Attach the default (available by default) security group.


To launch an instance

1. Open the Amazon EC2 console at https://console.aws.amazon.com/ec2/
    <img width="1366" height="615" alt="image" src="https://github.com/user-attachments/assets/147bdf3b-6024-4192-b81b-41a34df5696a" />

2. From the EC2 console dashboard, in the **Launch instance** pane, choose **Launch instance**.
   <img width="338" height="253" alt="image" src="https://github.com/user-attachments/assets/d9748fbc-45c5-473f-a5af-a75b068cf697" />

3. Under Name and tags, for Name, enter a descriptive name for your instance. The name of the instance must be `devops-ec2`.
   <img width="859" height="130" alt="image" src="https://github.com/user-attachments/assets/901546e3-5d6a-4099-af76-e215dfb0a882" />
   
5. Under Application and OS Images (Amazon Machine Image), choose the Amazon Linux AMI to launch this instance.
   <img width="859" height="475" alt="image" src="https://github.com/user-attachments/assets/66290062-4d4c-48f7-b71a-04b594e1b824" />

5. Under Instance type, select `t2.micro`.
   <img width="859" height="245" alt="image" src="https://github.com/user-attachments/assets/133d9321-f230-453c-87cd-6a5d07e11d25" />

6. Under Key pair (login), for Key pair name, choose an existing key pair or choose Create new key pair to create your first key pair. **For this task, create a new RSA key pair named `devops-kp`.**
   <img width="859" height="189" alt="image" src="https://github.com/user-attachments/assets/13aeca86-e46e-4147-828b-fb3e33a0a688" />

7. Under Network settings, notice that we selected your default VPC, selected the option to use the default subnet in an Availability Zone that we choose for you, and configured a security group with a rule that allows connections to your instance from anywhere (0.0.0.0.0/0). **For this task, Attach the default (available by default) security group.**
   <img width="862" height="447" alt="image" src="https://github.com/user-attachments/assets/86a53719-7636-45d8-a64f-399e37be5168" />

8. Review a summary of your instance configuration in the Summary panel, and when you're ready, choose Launch instance.
   <img width="417" height="454" alt="image" src="https://github.com/user-attachments/assets/118dafda-3c41-4210-9189-05e1178ac1d1" />

9. If the launch is successful, choose the ID of the instance from the Success notification to open the Instances page and monitor the status of the launch.
    <img width="1132" height="137" alt="image" src="https://github.com/user-attachments/assets/8d45a06b-d153-4396-a5c0-a8b9e5ee062a" />



   


   

