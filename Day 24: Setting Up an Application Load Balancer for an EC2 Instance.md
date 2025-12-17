# Day 24: Setting Up an Application Load Balancer for an EC2 Instance

The Nautilus DevOps team is currently working on setting up a simple application on the AWS cloud. They aim to establish an Application Load Balancer (ALB) in front of an EC2 instance where an Nginx server is currently running. While the Nginx server currently serves a sample page, the team plans to deploy the actual application later.

- Set up an Application Load Balancer named `xfusion-alb`.
- Create a target group named `xfusion-tg`.
- Create a security group named `xfusion-sg` to open port `80` for the public.
- Attach this security group to the ALB.
- The ALB should route traffic on port `80` to port `80` of the `xfusion-ec2` instance.
- Make appropriate changes in the default security group attached to the EC2 instance if necessary.

### To create an Application Load Balancer

- Open the Amazon EC2 console at [https://console.aws.amazon.com/ec2/](https://console.aws.amazon.com/ec2/)
- In the navigation pane, choose `Load Balancers`.

  <img width="208" height="204" alt="image" src="https://github.com/user-attachments/assets/8f8e3e7c-6f90-4756-b3d8-b9f0f6e69322" />

- Choose `Create load balancer`.

  <img width="351" height="62" alt="image" src="https://github.com/user-attachments/assets/b1e95699-30ac-41a0-ab14-2d6bd09a4dc5" />

- Under `Application Load Balancer`, choose `Create`.

  <img width="353" height="209" alt="image" src="https://github.com/user-attachments/assets/52049a5f-f6c1-4ec8-a25a-63db591f2a69" />

- `Basic configuration`
   - For `Load balancer name`, enter a name for your load balancer.

     <img width="898" height="148" alt="image" src="https://github.com/user-attachments/assets/619e4961-e588-4ccc-803a-56d00e405b9c" />

   - For `Scheme`, choose `Internet-facing` or `Internal`. An internet-facing load balancer routes requests from clients to targets over the internet. An internal load balancer routes requests to targets using private IP addresses.
 
     <img width="906" height="174" alt="image" src="https://github.com/user-attachments/assets/743eab99-d069-47c4-864e-8a8f92317228" />
     
   - For `Load balancer IP` address type, choose `IPv4`.

     <img width="1199" height="198" alt="image" src="https://github.com/user-attachments/assets/fe62717c-a56b-41b4-8c7d-52ef546008a2" />

- `Network mapping`
   - For `VPC`, select the VPC that you prepared for your load balancer. With an `internet-facing load balancer`, only VPCs with an `internet gateway` are available for selection.

     <img width="863" height="148" alt="image" src="https://github.com/user-attachments/assets/2e2fb064-e155-4c9f-8610-c7b868206b1e" />

   - For `Availability Zones` and `subnets`, enable zones for your load balancer as follows:
     - Select subnets from at least `two Availability Zones`

       **`Must ensure you select the subnet your ec2 instance is hosting`**
       
       <img width="1294" height="252" alt="image" src="https://github.com/user-attachments/assets/ed73a354-8b82-4f04-815a-edace2ef8c4c" />
       
- `Security groups`
   - Choose `create a new security group` to create one.
     
     <img width="1299" height="208" alt="image" src="https://github.com/user-attachments/assets/19441aa7-4076-4114-85fb-7362afa58b9e" />

   - Create a security group named `xfusion-sg`. 

     <img width="1346" height="393" alt="image" src="https://github.com/user-attachments/assets/ffd73be5-7a18-4a6f-8504-88482ab1d35a" />
 
   - Open port `80` for the `public`.

     <img width="1299" height="236" alt="image" src="https://github.com/user-attachments/assets/c62a03af-bd96-43d2-8e58-48a52960e214" />

   - Choose, newly created security group.

     <img width="1299" height="213" alt="image" src="https://github.com/user-attachments/assets/d079ba6b-aa1f-4730-9c69-d9ed3f7f5db6" />

- Create a `target group` named `xfusion-tg`.

  <img width="972" height="475" alt="image" src="https://github.com/user-attachments/assets/d8ea0cf6-a017-49a9-87c8-0c74160b47b6" />
  <img width="961" height="450" alt="image" src="https://github.com/user-attachments/assets/4bf33aa6-d496-4272-9000-00e4d95b8dae" />

  - Select target instance.

    <img width="1072" height="218" alt="image" src="https://github.com/user-attachments/assets/6a9ba96d-5bba-451d-95d7-7c8f0f127317" />
    
    - After selecting instance, click `Include as pending below`
      
    <img width="1013" height="188" alt="image" src="https://github.com/user-attachments/assets/eb45465d-e20e-4829-83d8-4ca9b2e55b2b" />

    - Review and click `Create target group` from step 3.

    <img width="1344" height="466" alt="image" src="https://github.com/user-attachments/assets/eceab429-8f64-4d41-a0bf-543ecac977ab" />

  - Verify `Load Balancer` working correctly from browser by entering load balancer `DNS name`.

    <img width="541" height="65" alt="image" src="https://github.com/user-attachments/assets/c8d3f9ae-889d-4bb0-8711-e3e3f0f3b438" />

- Entering `DNS name` should return `Nginx` welcome page.

  <img width="737" height="333" alt="image" src="https://github.com/user-attachments/assets/bbd2be52-2db0-4b8a-a42e-683bd8791e68" />

    





