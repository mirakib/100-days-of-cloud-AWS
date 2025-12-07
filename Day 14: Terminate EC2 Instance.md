# Day 14: Terminate EC2 Instance

During the migration process, several resources were created under the AWS account. Later on, some of these resources became obsolete as alternative solutions were implemented. Similarly, there is an instance that needs to be deleted as it is no longer in use.

- Delete the ec2 instance named `xfusion-ec2` present in `us-east-1` region.

- Before submitting your task, make sure instance is in `terminated` state.

AWS Docs to be followed: [Terminate Amazon EC2 instances](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/terminating-instances.html)

## To terminate an instance using the default terminate method

Open the Amazon EC2 console at https://console.aws.amazon.com/ec2/

- In the navigation pane, choose `Instances`.

  <img width="223" height="441" alt="image" src="https://github.com/user-attachments/assets/2a0d11ee-bf24-4961-aaf6-c507797d491a" />

- Select the instance, and choose `Instance state, Terminate (delete)` instance.

  <img width="1108" height="240" alt="image" src="https://github.com/user-attachments/assets/6eac3e6f-c34b-4c4e-b256-c26ba4bb7990" />

- Choose `Terminate (delete)` when prompted for confirmation.

  <img width="820" height="399" alt="image" src="https://github.com/user-attachments/assets/6eee55ef-7617-462c-ad6d-ba58e1bfe497" />

- After you terminate an instance, it remains visible for a short while, with a state of terminated.

  <img width="1103" height="202" alt="image" src="https://github.com/user-attachments/assets/99013b80-d0e3-4f94-96cc-875d421d0e4c" />

- If termination fails or if a terminated instance is visible for more than a few hours, see Terminated instance still displayed.

