# Day 5: Create GP3 Volume

For this task, Create a volume with the following requirements:

Name of the volume should be `datacenter-volume`.

- Volume type must be gp3.

- Volume size must be 2 GiB.


1. Open the Amazon EC2 console at https://console.aws.amazon.com/ec2/

2. In the navigation pane, choose Volumes and then choose Create volume.

3. (Outpost customers only) For Outpost ARN, enter the ARN of the AWS Outpost on which to create the volume.

4. For Volume type, choose the type of volume to create. For more information about the available volume types, see Amazon EBS volume types.

5. For Size, enter the size of the volume, in GiB. For more information, see Amazon EBS volume constraints.

6. (For io1, io2, and gp3 only) For IOPS, enter the maximum number of input/output operations per second (IOPS) that the volume should provide.

7. (For gp3 only) For Throughput, enter the throughput that the volume should provide, in MiB/s.

8. For Availability Zone, choose the Availability Zone in which to create the volume.
