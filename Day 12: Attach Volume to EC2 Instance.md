# Day 12: Attach Volume to EC2 Instance

An instance named **`xfusion-ec2`** and a volume named **`xfusion-volume`** already exists in **`us-east-1`** region. Attach the **`xfusion-volume`** volume to the **`xfusion-ec2`** instance, make sure to set the device name to **`/dev/sdb`** while attaching the volume.

## To attach an EBS volume to an instance
1. Open the Amazon EC2 console at https://console.aws.amazon.com/ec2/
2. In the navigation pane, choose **`Volumes`**.
3. Select the volume to attach and choose **`Actions, Attach volume`**.
4. For **`Instance`**, enter the ID of the instance or select the instance from the list of options.
5. For **`Device name`**, do one of the following:
    - For a root volume, select the required device name from the **`Reserved for root volume`** section of the list. Typically `/dev/sda1` or `/dev/xvda` for Linux instances depending on the AMI, or `/dev/sda1` for Windows instances.
    - For data volumes, select an available device name from the **`Recommended for data volumes`** section of the list.
    - To use a custom device name, select Specify a **`custom device name`** and then enter the device name to use.
    - 
<img width="1366" height="500" alt="image" src="https://github.com/user-attachments/assets/32350d4c-e676-4e53-9b5d-dda9a917a976" />

6. Choose **`Attach volume`**.
7. Connect to the instance and mount the volume.


AWS Docs to be followed: [Attach an Amazon EBS volume to an Amazon EC2 instance](https://docs.aws.amazon.com/ebs/latest/userguide/ebs-attaching-volume.html)
