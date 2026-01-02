# Day 40: Troubleshooting Internet Accessibility for an EC2-Hosted Application

The Nautilus Development Team recently deployed a new web application hosted on an EC2 instance within a public VPC named `nautilus-vpc`. The application, running on an Nginx server, should be accessible from the internet on port 80. Despite configuring the security group `nautilus-sg` to allow traffic on port 80 and verifying the EC2 instance settings, the application remains inaccessible from the internet. The team suspects that the issue might be related to the VPC configuration, as all other components appear to be set up correctly. The DevOps team has been asked to troubleshoot and resolve the issue to ensure the application is accessible to external users.

As a member of the Nautilus DevOps Team, your task is to perform the following:

- **Verify VPC Configuration**: Ensure that the VPC `nautilus-vpc` is properly configured to allow internet access.

- **Ensure Accessibility**: Make sure the EC2 instance `nautilus-ec2` running the `Nginx` server is accessible from the internet on port `80`.

## Inspect and identify the problem

1. **Inspect vpc resource map**
The problem of the task is that `nautilus-vpc` has no internet gateway attached to `nautilus-subnet` route table `nautilus-rtb`. Reason why instance could not connect to internet because the subnet it is in, acting as a private subnet.

<img width="1045" height="180" alt="image" src="https://github.com/user-attachments/assets/b33f2b01-6dc7-4eaf-a46e-74043cac88e8" />

2. **Inspect subnet route table status**

We can see the route table indeed has a internet gateway but with `Blackhole` status. Meaning the internet gateway is not `Active`. Thus the instance could not accept connection from internet.

<img width="1119" height="154" alt="image" src="https://github.com/user-attachments/assets/71c90403-6ea9-40cb-a331-23439ca16e30" />

2. **Inspect internet gateways**

Go to `Internet Gateways` from vpc navigation pane and you will see that the internet gateway route table `nautilius-rtb` trying to point is currently not attached to any vpc. Thus it has state `Detached`.

<img width="1149" height="167" alt="image" src="https://github.com/user-attachments/assets/0d44cacf-d745-4453-ac34-5cbeba9379bf" />

## Ensure Accessibility

1. Attached internet gateway `nautilus-ig` to `nautilus-vpc`.

   - Select internet gateway `nautilus-ig` and click `Action` to choose `Attach to vpc` > `nautilus-vpc`.

     <img width="1286" height="272" alt="image" src="https://github.com/user-attachments/assets/faf88719-8db3-4a30-bcc2-eab7e6b54a47" />
  
   - Now check `nautilus-rtb` routes now and you will see the internet gateway status is `Active`.

     <img width="1119" height="154" alt="image" src="https://github.com/user-attachments/assets/d1b61b08-aeca-4ee1-bd83-4e38c918f42d" />

   - Also check `nautilius-vpc` resource map.

     <img width="1043" height="174" alt="image" src="https://github.com/user-attachments/assets/922ce6f9-c72d-425c-988c-62e7b3a8e970" />

## Verify Accessibility

Go to EC2 dashboard and select `nautilius-ec2` and enter it's public ip to browser.

<img width="1358" height="318" alt="image" src="https://github.com/user-attachments/assets/11e4611e-15c5-4619-adfd-e574ea484c1a" />

You will able to see Nginx welocme page.
