# Day 15: Create Volume Snapshot

Create a snapshot of an existing volume named `datacenter-vol` in `us-east-1` region.

- The name of the snapshot must be `datacenter-vol-ss`.
- The description must be `datacenter Snapshot`.
- Make sure the snapshot status is `completed` before submitting the task.

AWS Docs to be followed: [Create a snapshot of an EBS volume](https://docs.aws.amazon.com/ebs/latest/userguide/ebs-create-snapshot.html)

## To create a snapshot

- Open the Amazon EC2 console at `https://console.aws.amazon.com/ec2/`
- In the navigation pane, choose `Snapshots, Create snapshot`.

  <img width="213" height="124" alt="image" src="https://github.com/user-attachments/assets/aa6eeef3-8aec-445e-a3b6-b51393a556b6" />

- For `Resource type`, choose `Volume`.

  <img width="1299" height="152" alt="image" src="https://github.com/user-attachments/assets/44be6f1b-e00e-4014-b803-f35a113ccfeb" />

- For `Volume ID`, select the volume from which to create the snapshot. The Encryption field indicates the volume and resulting snapshot's encryption status. It can't be modified.

  <img width="1299" height="266" alt="image" src="https://github.com/user-attachments/assets/9fa1b251-d15b-4622-9681-04d8e705a1be" />

- (Optional) For `Description`, enter a brief description for the snapshot.

  <img width="1299" height="226" alt="image" src="https://github.com/user-attachments/assets/9c72ca0a-605b-49f9-8d65-ecad342bd2e6" />

- (Optional) To assign custom tags to the snapshot, in the `Tags` section, choose `Add tag`, and then enter the key-value pair. You can add up to 50 tags.

  <img width="1299" height="189" alt="image" src="https://github.com/user-attachments/assets/f819cb0f-64a4-45c4-84ba-2263c2435e20" />

- Choose `Create snapshot`.
  
  <img width="146" height="34" alt="image" src="https://github.com/user-attachments/assets/6d4e1d37-f05d-44c9-86de-8b7810db10c8" />

- After creating the snapshot, rename it from `Name` column.

  <img width="1108" height="147" alt="image" src="https://github.com/user-attachments/assets/0d51b393-5c4f-4469-b3f0-e1b9f5df61b4" />

- Verify the snapshot status for `completed`

  <img width="1108" height="143" alt="image" src="https://github.com/user-attachments/assets/21035ffd-63a0-43ef-a1ac-ec2ef4cee67b" />

