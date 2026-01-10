# Day 45: Configure NAT Gateway for Internet Access in a Private VPC

The Nautilus DevOps team is tasked with enabling internet access for an EC2 instance running in a private subnet. This instance should be able to upload a test file to a public S3 bucket once it can access the internet. To achieve this, the team must set up a NAT Gateway in a public subnet within the same VPC.

1) A VPC named `xfusion-priv-vpc` and a private subnet `xfusion-priv-subnet` have already been created.
2) An EC2 instance named `xfusion-priv-ec2` is already running in the private subnet.
3) The EC2 instance is configured with a cron job that uploads a test file to a `bucket xfusion-nat-11088` once internet is accessible.

Your task is to:

- Create a public subnet named `xfusion-pub-subnet` in the same VPC.
- Create an Internet Gateway and attach it to the VPC.
- Create a route table `xfusion-pub-rt` and associate it with the public subnet.
- Allocate an Elastic IP and create a NAT Gateway named `xfusion-natgw`.
- Update the private route table to route `0.0.0.0/0` traffic via the NAT Gateway.

Once complete, verify that the EC2 instance can reach the internet by confirming the presence of the test file in the S3 `bucket xfusion-nat-11088`. After completing all the configuration, please wait a few minutes for the test file to appear in the bucket, as it may take 2–3 minutes.


## Task 1: Create a public subnet

1. **From VPC dashboard, select subnet and click `Create`.**
2. **Select `xfusion-priv-vpc`.**

   <img width="1318" height="222" alt="image" src="https://github.com/user-attachments/assets/009acfa2-4f4e-42cb-89fe-e41384d337ca" />

3. **Under `Subnet settings` section, enter subnet name and select valid CIDR block.**

   <img width="1318" height="462" alt="image" src="https://github.com/user-attachments/assets/fbe3e19c-85f9-4ef2-8a94-7c69a51e8cb7" />


## Task 2: Create an Internet Gateway and attach it to the VPC

1. **From VPC dashboard, create an internet gateway with any relavant name.**

   <img width="1312" height="436" alt="image" src="https://github.com/user-attachments/assets/f8cc4b78-7069-44ce-9ed1-498a5510a31f" />

2. **Preview all internet gaetways.**

   <img width="1149" height="171" alt="image" src="https://github.com/user-attachments/assets/aad3452e-466a-4662-96c5-10fc5a23ef6a" />

   **You can see that newly created internet gateway is detached.**

3. **To attach the internet gateway to `xfusion-priv-vpc`, select `xfusion-ig` and choose `Action` > `Attach`. and select `xfusion-priv-vpc`**

   <img width="1298" height="262" alt="image" src="https://github.com/user-attachments/assets/b3878b30-65c8-4826-a051-819f9af50d48" />


## Task 3: Create a route table

1. **Create a route table in `xfusion-priv-vpc`**

   <img width="1328" height="440" alt="image" src="https://github.com/user-attachments/assets/3c7e0bd7-a114-4ea2-aefa-dcae1f2810fa" />

2. **After creating route table, edit its route to internet gateway `xfusion-ig` you just created.**

   <img width="1366" height="356" alt="image" src="https://github.com/user-attachments/assets/c0433b06-e92d-4aed-99de-e57bf4ee22d5" />

3. **Next, change `xfusion-pub-subnet` route table association to `xfusion-pub-rt`.**

   <img width="1365" height="428" alt="image" src="https://github.com/user-attachments/assets/33bed471-690b-4241-be49-e07345b92997" />

4. **Now select `xfusion-priv-vpc` and see its resource map.**

   <img width="935" height="182" alt="image" src="https://github.com/user-attachments/assets/53cac65e-8d12-4808-9d38-19e776e0fbcc" />

   **You can notice that `xfusion-pub-subnet` is showing as public subnet now.**

## Task 4: Allocate an Elastic IP (Optional)

1. **From VPC dashboard menu, select `Elastic IPs` and click `Allocate Elastic IP Address`.**

   <img width="1125" height="200" alt="image" src="https://github.com/user-attachments/assets/e4b8364d-27a9-480a-b21b-cef1b3d48d7c" />

>[!Note]
> **Elastic IP can be allocated during NAT Gateway creation too. (PREFFERED)**

## Task 5: Create a NAT Gateway

1. **From VPC dashboard menu, select `NAT Gateways` and click `Create NAT gateways`.**

   <img width="1125" height="119" alt="image" src="https://github.com/user-attachments/assets/2805aa25-1986-41d2-8d42-04936c28e49b" />

2. **Enter gateway name and select `Availability mode` as `Zonal` and `xfusion-pub-subnet`.**

   <img width="1294" height="471" alt="image" src="https://github.com/user-attachments/assets/ca4f4c36-697a-4722-a38e-4f941907eb49" />

3. **Wait until `Status` = `Available`.**
   
   <img width="1108" height="340" alt="image" src="https://github.com/user-attachments/assets/c7972fe2-38b6-4a19-870b-04c3d452dd79" />


## Task 6: Update the private route table to route 0.0.0.0/0 traffic via the NAT Gateway.

1. **Go to **VPC** > **Route Tables** and identify the route table associated with: `xfusion-priv-subnet`**

   <img width="1173" height="196" alt="image" src="https://github.com/user-attachments/assets/76383851-16be-4441-b53e-79c6c94d7083" />

2. **Also associate private route table with private subnet by selecting the private route table's `Edit subnet associate` tab.**

   <img width="1149" height="194" alt="image" src="https://github.com/user-attachments/assets/ff20cf3a-673b-4c1f-b8b7-4223c5f32275" />
  
3. Go to **Routes** > **Edit routes and update the private route table to route `0.0.0.0/0` traffic to the NAT Gateway.**

   <img width="1332" height="241" alt="image" src="https://github.com/user-attachments/assets/dae7dbf8-091e-4b3c-8125-0283ac72414a" />



   
## Task 7: Verify instance can access S3 Bucket

1. **Go to S3 Bucket and select the existing bucket under `General purpose bucket` section.**

   <img width="1018" height="184" alt="image" src="https://github.com/user-attachments/assets/e653cc35-b811-4577-8805-cf5de567947c" />

2. **Click on the bucket and you should see the test file uploaded by the EC2 instance.**

    <img width="1063" height="446" alt="image" src="https://github.com/user-attachments/assets/6dc28e25-9294-4aa0-9de7-6e7c43f0147c" />

