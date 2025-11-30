# Day 5: Create GP3 Volume

For this task, Create a volume with the following requirements:

Name of the volume should be `datacenter-volume`.

- Volume type must be gp3.

- Volume size must be 2 GiB.


1. Open the Amazon EC2 console at https://console.aws.amazon.com/ec2/

   <img width="1366" height="615" alt="image" src="https://github.com/user-attachments/assets/5485a2ad-3a5d-480d-80cf-8e5f86728674" />

3. In the navigation pane, choose Volumes and then choose `Create volume`.

   <img width="1108" height="163" alt="image" src="https://github.com/user-attachments/assets/5edfb848-61f8-43a8-a169-2774f82a1160" />

5. (Outpost customers only) For Outpost ARN, enter the ARN of the AWS Outpost on which to create the volume.

6. For Volume type, choose the type of volume to create. For more information about the available volume types, see Amazon EBS volume types.

7. For Size, enter the size of the volume, in GiB. For more information, see Amazon EBS volume constraints.

8. (For io1, io2, and gp3 only) For IOPS, enter the maximum number of input/output operations per second (IOPS) that the volume should provide.

9. (For gp3 only) For Throughput, enter the throughput that the volume should provide, in MiB/s.

10. For Availability Zone, choose the Availability Zone in which to create the volume.

11. Choose `Create volume`.
  
12. After creating the volume, rename it with `datacenter-volume` [For the task only]

13. To use the volume, wait for it to reach the available state and then attach it to an Amazon EC2 instance in the same Availability Zone.
