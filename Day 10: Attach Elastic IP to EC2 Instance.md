# Day 10: Attach Elastic IP to EC2 Instance

There is an instance named devops-ec2 and an elastic-ip named devops-ec2-eip in us-east-1 region. Attach the devops-ec2-eip elastic-ip to the devops-ec2 instance.

AWS Documentation to be folllowed: [Associate an Elastic IP address](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/working-with-eips.html#using-instance-addressing-eips-associating)

To associate an Elastic IP address with an instance

1. Open the Amazon EC2 console at https://console.aws.amazon.com/ec2/

3. In the navigation pane, choose **Elastic IPs**.
  
6. Select the Elastic IP address to associate and choose **Actions, Associate Elastic IP address**.

7. For **Resource type**, choose **Instance**.

8. For instance, choose the instance with which to associate the Elastic IP address. You can also enter text to search for a specific instance.

9. (Optional) For **Private IP** address, specify a private IP address with which to associate the Elastic IP address.

10. Choose **Associate**.
