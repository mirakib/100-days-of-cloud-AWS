# Day 4: Allocate Elastic IP

For this task, allocate an Elastic IP address, name it as nautilus-eip

`Note : Before you use an Elastic IP, you must allocate one for use in your VPC.`

**To allocate an Elastic IP address**

1. Open the Amazon VPC console at https://console.aws.amazon.com/vpc/

   <img width="1366" height="615" alt="image" src="https://github.com/user-attachments/assets/320e1824-c189-4582-9bfd-388d8cbde37d" />

3. In the navigation pane, choose Elastic IPs.

   <img width="1366" height="615" alt="image" src="https://github.com/user-attachments/assets/25e24134-9132-42d4-bad9-8014b50bc90f" />

5. Choose Allocate Elastic IP address.

6. (Optional) When you allocate an Elastic IP address (EIP), you choose the Network border group in which to allocate the EIP. A network border group is a collection of Availability Zones (AZs), Local Zones, or Wavelength Zones from which AWS advertises a public IP address. Local Zones and Wavelength Zones may have different network border groups than the AZs in a Region to ensure minimum latency or physical distance between the AWS network and the customers accessing the resources in these Zones.
Important

    `Note: You must allocate an EIP in the same network border group as the AWS resource that will be associated with the EIP. An EIP in one network border group can only be advertised in zones in that network border group and not in any other zones represented by other network border groups.`

4. For Public IPv4 address pool choose one of the following:

   - **Amazon's pool of IP addresses**—If you want an IPv4 address to be allocated from Amazon's pool of IP addresses.

   - **My pool of public IPv4 addresses**—If you want to allocate an IPv4 address from an IP address pool that you have brought to your AWS account. This option is disabled if you do not have any IP address pools.

   - **Customer owned pool of IPv4 addresses**—If you want to allocate an IPv4 address from a pool created from your on-premises network for use with an Outpost. This option is only available if you have an Outpost.

     <img width="1366" height="615" alt="image" src="https://github.com/user-attachments/assets/eb2db9ce-442b-4017-9f16-ec7bb6a9c580" />

5. (Optional) Add or remove a tag.

[Add a tag] Choose **Add new tag** and do the following:

   - For **Key**, enter the key name.

   - For **Value**, enter the key value.

     <img width="1315" height="171" alt="image" src="https://github.com/user-attachments/assets/8fe6daa6-522d-4b62-8922-61eabcc27b0d" />

[Remove a tag] Choose **Remove** to the right of the tag’s Key and Value.

6. Choose **Allocate.**
   
   <img width="203" height="66" alt="image" src="https://github.com/user-attachments/assets/4db2157a-f833-4d90-b750-21654d901f7b" />

7. After allocation, click on pen icon to rename it with `nautilus-eip`

   <img width="1109" height="194" alt="image" src="https://github.com/user-attachments/assets/b8a36b86-da00-4658-9205-4de13ef03604" />

8. After renaming

   <img width="1107" height="189" alt="image" src="https://github.com/user-attachments/assets/b600d33d-1f7e-42e6-8327-d7f93f2f7936" />

Fore more, AWS Docs: [Start using Elastic IP addresses](https://docs.aws.amazon.com/vpc/latest/userguide/WorkWithEIPs.html#transfer-EIPs-intro
)

