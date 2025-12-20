# Day 27: Configuring a Public VPC with an EC2 Instance for Internet Access

The Nautilus DevOps Team has received a request from the Networking Team to set up a new public VPC to support a set of public-facing services. This VPC will host various resources that need to be accessible over the internet. As part of this setup, you need to ensure the VPC has public subnets with automatic IP assignment for resources. Additionally, a new EC2 instance will be launched within this VPC to host public applications that require SSH access. This setup will enable the Networking Team to deploy and manage public-facing applications.

Create a public VPC named `xfusion-pub-vpc`, and a subnet named `xfusion-pub-subnet` under the same, make sure public IP is being auto assigned to resources under this subnet. Further, create an EC2 instance named `xfusion-pub-ec2` under this VPC with instance type `t2.micro`. Make sure `SSH` port `22` is open for this instance and accessible over the internet.

### To create a VPC, subnets, and other VPC resources using the console

- On the VPC dashboard, choose `Create VPC`.
- For Resources to create, choose `VPC and more`.

  <img width="483" height="146" alt="image" src="https://github.com/user-attachments/assets/b95e449a-3f8f-4974-801d-a1372153b0be" />

- Keep Name tag auto-generation selected to create Name tags for the VPC resources or clear it to provide your own Name tags for the VPC resources.

  <img width="484" height="142" alt="image" src="https://github.com/user-attachments/assets/f30ddc61-02c6-40f3-953e-d7b90744b733" />

- For `IPv4 CIDR block`, enter an IPv4 address range for the VPC. A VPC must have an IPv4 address range.

  <img width="483" height="191" alt="image" src="https://github.com/user-attachments/assets/224cf0e5-a641-4065-a6ad-eede614d35c9" />

- For `Number of Availability Zones` (AZs), Keep Zone to one and one public subnet.

  <img width="482" height="415" alt="image" src="https://github.com/user-attachments/assets/ac4409a3-f6c9-462f-9090-fbd285231b06" />

- Rename the subnet with `xfusion-pub-subnet`.

  <img width="1108" height="306" alt="image" src="https://github.com/user-attachments/assets/727a3f2c-c858-4e52-81ef-a9a1432e6115" />

- Now create an EC2 instance name `xfusion-pub-ec2` with needed requirments under the public subnet of the newly created vpc.

  <img width="1054" height="458" alt="image" src="https://github.com/user-attachments/assets/32d3535b-bc66-4dee-8417-af10d7644476" />


