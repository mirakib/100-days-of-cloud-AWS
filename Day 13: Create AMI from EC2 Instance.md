# Day 13: Create AMI from EC2 Instance

For this task, create an AMI from an existing EC2 instance named `devops-ec2` with the following requirement:

Name of the AMI should be `devops-ec2-ami`, make sure AMI is in available state.

AWS Documentation to be folllowed: [Creating an AMI from an Amazon EC2 Instance](https://docs.aws.amazon.com/toolkit-for-visual-studio/latest/user-guide/tkv-create-ami-from-instance.html)

## Creating an AMI from an existing Amazon EC2 instance

  - From the `AWS Toolkit Explorer`, expand `Amazon EC2` and choose `Instances` to view a list of your existing instances.
  - Right-click the instance that you want to use as the basis for your AMI and choose `Create Image (ABS AMI)` to open the `Create Image dialog window`.
  - From the Create Image dialog window, add a name and a description for your image into the provided fields, then choose the `OK` button to continue.
  - The Image Created confirmation window opens in Visual Studio when the image is created, choose the OK button to continue.
