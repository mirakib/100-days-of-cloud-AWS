# Day 25: Setting Up an EC2 Instance and CloudWatch Alarm

The Nautilus DevOps team has been tasked with setting up an EC2 instance for their application. To ensure the application performs optimally, they also need to create a CloudWatch alarm to monitor the instance's CPU utilization. The alarm should trigger if the CPU utilization exceeds `90%` for one consecutive `5-minute period`. To send notifications, use the SNS topic named `devops-sns-topic` which is already created.

- Launch EC2 Instance: Create an EC2 instance named `devops-ec2` using any appropriate **Ubuntu AMI**.

- Create CloudWatch Alarm: Create a CloudWatch alarm named `devops-alarm` with the following specifications:
  - **Statistic**: Average
  - **Metric**: CPU Utilization
  - **Threshold**: >= 90% for 1 consecutive 5-minute period.
  - **Alarm Actions**: Send a notification to `devops-sns-topic`.

_**AWS Docs to be followed**: [Create a CloudWatch alarm for an instance](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-cloudwatch-createalarm.html)_

### To create an alarm using the Amazon EC2 console

- Open the Amazon EC2 console at [https://console.aws.amazon.com/ec2/](https://console.aws.amazon.com/ec2/)
- In the navigation pane, choose `Instances`.
- Select the instance and choose `Actions, Monitor and troubleshoot, Manage CloudWatch alarms`.

  <img width="1108" height="486" alt="image" src="https://github.com/user-attachments/assets/2a7d13ce-5586-4b0b-948d-929fe50fd92a" />

- On the `Manage CloudWatch alarms` detail page, under `Add or edit alarm`, select `Create an alarm`.

  <img width="1299" height="236" alt="image" src="https://github.com/user-attachments/assets/ef902656-de24-475a-be8e-d442bfa0a031" />

- For `Alarm notification`, choose whether to configure Amazon Simple Notification Service (Amazon SNS) notifications. Enter an existing Amazon SNS topic or enter a name to create a new topic.

  <img width="1299" height="129" alt="image" src="https://github.com/user-attachments/assets/89779b3d-3505-48bf-a5ee-6014efa4bd27" />

- For `Alarm action`, choose whether to specify an action to take when the alarm is triggered. Choose an action from the list.
- For `Alarm thresholds`, select the metric and criteria for the alarm. For example, to create an alarm that is triggered when CPU utilization reaches 90% for a 5 minute period, do the following:

  - Keep the default setting for `Group samples by (Average)` and `Type of data to sample (CPU utilization)`.

  - Choose >= for `Alarm when` and enter `90` for Percent.

  - Enter 1 for `Consecutive period` and select `5 minutes` for `Period`.

  <img width="1299" height="435" alt="image" src="https://github.com/user-attachments/assets/2e16b842-1487-40b9-9676-4d70ce03b2d9" />


- (Optional) For Sample metric data, choose `Add to dashboard`.
- Choose `Create`.
