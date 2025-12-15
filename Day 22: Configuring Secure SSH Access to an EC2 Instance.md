# Day 22: Configuring Secure SSH Access to an EC2 Instance

The Nautilus DevOps team needs to set up a new EC2 instance that can be accessed securely from their landing host (`aws-client`). The instance should be of type `t2.micro` and named `nautilus-ec2`. A new SSH key should be created on the `aws-client` host under the`/root/.ssh/` folder, if it doesn't already exist. This key should then be added to the `root` user's authorised keys on the EC2 instance, allowing passwordless SSH access from the `aws-client` host.

1. Create RSA key in AWS client.

   ```bash
   ssh-keygen -t rsa
   ```

    <img width="678" height="407" alt="image" src="https://github.com/user-attachments/assets/aab67fc9-c8e4-46e4-9931-417bc7078ae7" />

2. `Import key pair` in AWS console with client public key.

3. Launch instace from EC2 dashboard and add imported key on step 2.

   <img width="859" height="188" alt="image" src="https://github.com/user-attachments/assets/9649d603-093f-411d-902f-f4307a5565ea" />

4. SSH into EC2 instance from AWS client.

   ```bash
   ssh ec2-user@<public-ip>
   ```

5. In `sudo` mode, Go to `/root/.ssh` directory and add aws client public key to `authorized_keys`
6. Try login as root into the EC2 instance

   ```bash
   ssh root@<public-ip>
   ```


   
