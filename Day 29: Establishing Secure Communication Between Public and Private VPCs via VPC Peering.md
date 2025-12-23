# Day 29: Establishing Secure Communication Between Public and Private VPCs via VPC Peering

The Nautilus DevOps team has been tasked with demonstrating the use of VPC Peering to enable communication between two VPCs. One VPC will be a private VPC that contains a private EC2 instance, while the other will be the default public VPC containing a publicly accessible EC2 instance.

1) There is already an existing **EC2 instance** in the public vpc/subnet:

- **Name**: `xfusion-public-ec2`

2) There is already an existing Private VPC:

- **Name**: `xfusion-private-vpc`
- **CIDR**: `10.1.0.0/16`

3) There is already an existing **Subnet** in `xfusion-private-vpc`:

- **Name**: `xfusion-private-subnet`
- **CIDR**: `10.1.1.0/24`

4) There is already an existing EC2 instance in the private subnet:

- **Name**: `xfusion-private-ec2`

5) Create a Peering Connection between the **Default VPC** and the **Private VPC**:

- **VPC Peering Connection Name**: `xfusion-vpc-peering`

6) **Configure Route Tables** to enable communication between the two VPCs.

- Ensure the private EC2 instance is accessible from the public EC2 instance.

7) Test the Connection:

- Add `/root/.ssh/id_rsa.pub` public key to the public EC2 instance's ec2-user's `authorized_keys` to make sure we are able to ssh into this instance from AWS client host. You may also need to update the security group of the private EC2 instance to allow `ICMP` traffic from the public/default VPC CIDR. This will enable you to ping the private instance from the public instance.
- SSH into the public EC2 instance and ensure that you can ping the private EC2 instance.


## Create VPC peering connection

1. Go to VPC dashboard. In the navigation pane, choose Peering connections.

   <img width="217" height="389" alt="image" src="https://github.com/user-attachments/assets/dbe1219e-64af-4e04-b854-ddbc5a7cecb6" />
   
3. Choose **Create peering connection**.

   <img width="406" height="67" alt="image" src="https://github.com/user-attachments/assets/b316d448-09ec-4edc-960f-3c545f41d0f4" />

5. (Optional) For Name, specify a name the VPC peering connection. This creates a tag with a key of Name and the value that you specify.

   <img width="1315" height="136" alt="image" src="https://github.com/user-attachments/assets/083a75b4-c285-4c3d-b15a-3affa8bd9b88" />

7. For **VPC ID (Requester)**, select a VPC from the current account.

   <img width="1315" height="231" alt="image" src="https://github.com/user-attachments/assets/4cb9433d-bd97-406d-ad75-39cb5ba6be8d" />

8. Under Select another VPC to peer with, do the following:

    1. For **Account**, to peer with a VPC in another account, choose **Another account** and enter the account ID . Otherwise, keep My account.

    2. For **Region**, to peer with a VPC in another **Region**, choose **Another Region** and choose the **Region** . Otherwise, keep This Region.

    3. For **VPC ID (Accepter)**, select a VPC from the specified account and Region.

   <img width="1315" height="377" alt="image" src="https://github.com/user-attachments/assets/a032833e-4afe-4132-a465-e1b08888662d" />

9. Choose `Create peering connection`.

10. After creating peering connection, **Accept VPC peering connection request**.

    <img width="820" height="322" alt="image" src="https://github.com/user-attachments/assets/471b9b97-2426-429c-b9b8-7c4c5cef37d1" />

11. For DNS resolution, Go to VPC Peering, Seclect the peering connection, and select `Actions > Edit DNS settings` and allow both.

    <img width="1315" height="266" alt="image" src="https://github.com/user-attachments/assets/aab8fa96-e5a5-4564-8cc9-3b5ff0246372" />

11. Update the route tables for both VPCs to enable communication between them. For more information, see Update your route tables for a VPC peering connection.

    **Point each others CIDR to peering connection `xfusion-vpc-peering`**
    
    a) Change route table for `default`

    <img width="1074" height="206" alt="image" src="https://github.com/user-attachments/assets/0776b684-c029-45ca-9336-7c077e55021e" />

    b) Change route table for `xfusion-private-vpc`

    <img width="1074" height="175" alt="image" src="https://github.com/user-attachments/assets/5801c88d-87ae-4033-95fc-75958617e4f5" />

12. Change `Security Group` for `xfusion-public-ec2` and add inbound rule for `ICMP` to **private** vpc's CIDR as source.

    <img width="1315" height="389" alt="image" src="https://github.com/user-attachments/assets/339202a1-3ebd-4d83-956f-7f09ceec3aeb" />

12. Change `Security Group` for `xfusion-private-ec2` and add inbound rule for `ICMP` to **public** vpc's CIDR as source.

    <img width="1332" height="318" alt="image" src="https://github.com/user-attachments/assets/f93e7b04-96f5-490a-aac8-c011baed7064" />

## Enable SSH access from aws client to `xfusion-public-ec2`

1. Change security group of `xfusion-public-ec2` and add inbound rule for `SSH` from `Anywhre IPv4`.

   <img width="1074" height="201" alt="image" src="https://github.com/user-attachments/assets/b203937b-602b-438d-99c5-176b32f1bf56" />

2. Now connect to `xfusion-public-ec2` from `EC2 instance connect` using AWS management console.

3. Add **aws client** public key `id_rsa.pub` to `xfusion-public-ec2`s `authorized_keys`.
4. Verify you can ssh into `xfusion-public-ec2`.

   ```bash
   ssh ec2-user@<public-ip-of-xfusion-public-ec2>
   ```

## Verify you can ping to private instance using private ip from public instance.

1. ping from public instance to private instance using private ip

   <img width="584" height="204" alt="image" src="https://github.com/user-attachments/assets/cc259fd1-d736-4dd4-9692-5e99ec302e30" />



