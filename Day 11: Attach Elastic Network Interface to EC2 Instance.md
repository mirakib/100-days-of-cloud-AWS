# Day 11: Attach Elastic Network Interface to EC2 Instance

- An instance named **`nautilus-ec2`** and an elastic network interface named **`nautilus-eni`** already exists in **`us-east-1`** region.

- Attach the **`nautilus-eni`** network interface to the **`nautilus-ec2`** instance.

- Make sure status is attached before submitting the task.

### To attach a network interface using the Network Interfaces page

- Open the **Amazon EC2** console at https://console.aws.amazon.com/ec2/

- In the navigation pane, choose **Network Interfaces**.

  <img width="192" height="185" alt="image" src="https://github.com/user-attachments/assets/16f59970-c640-4872-a758-cb2b684b9e21" />

- Select the checkbox for the network interface.

  <img width="1108" height="171" alt="image" src="https://github.com/user-attachments/assets/0f417776-fc21-4d06-9903-edbcd039e945" />

- Choose **Actions, Attach**.

- Choose an instance. If the instance supports multiple network cards, you can choose a network card.

  <img width="600" height="314" alt="image" src="https://github.com/user-attachments/assets/f4d60509-628a-40f3-a4da-bc1e733c7368" />

- Choose **Attach**.

### AWS Docs to be followed: [Attach a network interface](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/network-interface-attachments.html)
