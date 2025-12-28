# Day 35: Deploying and Managing Applications on AWS

The Nautilus DevOps team needs a new private RDS instance for their application. They need to set up a MySQL database and ensure that their existing EC2 instance can connect to it. This will help in managing their database needs efficiently and securely.

1) Task Details:

    - Create a private `RDS instance` named `nautilus-rds` using a `sandbox` template.
    - The engine type must be `MySQL v8.4.5`, and it must be a `db.t3.micro` type instance.
    - The **master username** must be `nautilus_admin` with an appropriate password.
    - The **RDS storage** type must be `gp2`, and the storage size must be `5GiB`.
    - **Create a database** named `nautilus_db`.
    - Keep the rest of the configurations as default. Ensure the instance is in `available` state.
    - **Adjust the security groups** so that the `nautilus-ec2` instance can connect to the RDS on port `3306` and also open port `80` for the instance.

2) An EC2 instance named `nautilus-ec2` exists. Connect to this instance from the AWS console. Create an `SSH` key (`/root/.ssh/id_rsa`) on the `aws-client` host if it doesn't already exist. Add the public key to the `authorized_keys` of the root user on the EC2 instance for password-less SSH access.

3) There is a file named `index.php` under the `/root` directory on the `aws-client` host. Copy this file to the `nautilus-ec2` instance under the `/var/www/html/` directory. Make the appropriate changes in the file to connect to the RDS.

4) You should see a `Connected successfully` message in the browser once you access the instance using the **public IP**.

   
## Task 1: Adjust security group of `nautilus-ec2`

1. Go to EC2 dashboard and select `Security groups` under `Network & Security` from navaigation pane.

   <img width="204" height="176" alt="image" src="https://github.com/user-attachments/assets/164e583a-41a6-4a6b-95bd-f89bb9bab8e9" />

2. Select the available default security group.

   <img width="1125" height="169" alt="image" src="https://github.com/user-attachments/assets/b6f9fa8d-9ba4-47bb-9d89-892287e7b1b8" />

3. Edit `Inbound rules` as follows:

   <img width="1318" height="385" alt="image" src="https://github.com/user-attachments/assets/bbd6a93c-6a91-4fb0-8a64-e81130c9b034" />


## Task 2: Enable SSH access to `nautilus-ec2` from `aws-client`

1. Generate SSH keys in `aws-client`.

   ```sh
   ssh-keygen -t rsa
   ```

2. Connect to `nautilus-ec2` using `EC2 Instance Connect`.

   <img width="1348" height="512" alt="image" src="https://github.com/user-attachments/assets/506c577f-b2da-4076-8e1a-243429128576" />

3. Add `aws-client` public key `id_rsa.pub` to `root` users `authorized_keys` of `nautilus-ec2`.

   - After SSH into `nautilus-ec2`, switch to `root` user.

     ```sh
     sudo su -
     ```
   - Move to `root` directory.

     ```sh
     cd ~
     ```
    
   - Now add `aws-client` public key to `.ssh/authorized_keys`.

     ```sh
     nano .ssh/authorized_keys
     ```

4. Now SSH into `nautilus-ec2` from `aws-client` as `root`.

   ```sh
   ssh root@<nautilus-ec2-pub-ip>
   ```

# Task 3: Create a private RDS instance

1. Sign in to the AWS Management Console and open the Amazon RDS console and click `Create a database`.

   <img width="716" height="164" alt="image" src="https://github.com/user-attachments/assets/b67671a8-b41b-4401-87df-36f709d192c7" />

2. In the `Choose a database creation method ` pane, choose `Full configuration`.

   <img width="1299" height="146" alt="image" src="https://github.com/user-attachments/assets/d3eede49-b84d-476e-85d4-9704199b4c14" />

3. Next go to `Engine Options` pane and select engine type as `MySQL`.

   <img width="1299" height="411" alt="image" src="https://github.com/user-attachments/assets/5f889ecc-5a35-404b-a801-b407491ec822" />

4. On the same tab, choose `Engine version` as `MySQL v8.4.5`.

   <img width="1291" height="157" alt="image" src="https://github.com/user-attachments/assets/a8ab8e82-71c0-462f-b149-bdabe202a443" />

5. Next in the `Templates` tab, select `Free tier`.

   <img width="1299" height="166" alt="image" src="https://github.com/user-attachments/assets/46da9890-8b87-4e48-b48a-141ef4f5d2f6" />

6. Next in the `Availability and durability` tab, choose `Single-AZ DB instance deployment (1 instance)`.

   <img width="1303" height="476" alt="image" src="https://github.com/user-attachments/assets/48b0dd3d-d015-4f3a-a9d4-110aa910f6e5" />

7. Next on the `Settings` tab, enter instance name and master username.

   <img width="1303" height="281" alt="image" src="https://github.com/user-attachments/assets/ee0f48bd-12b9-4c8f-b382-5603fcd16de8" />

8. On the same tab, enter appopriate master password.

   <img width="1296" height="313" alt="image" src="https://github.com/user-attachments/assets/69647636-0be8-46f7-b54f-efcd122239f1" />

9. Next in the `Instance configuration` tab, select `DB-instnce class` as `Burstable classes (includes t classes)` and `db.t3.micro`.

    <img width="1303" height="336" alt="image" src="https://github.com/user-attachments/assets/570d41ce-d242-437c-be6f-cb297e3982ff" />

10. In the next `Storage` tab, select `Storage type` as `gp2` and `Allocate storage` as `5`.

    <img width="1303" height="274" alt="image" src="https://github.com/user-attachments/assets/fb7eab1b-9231-4172-b564-5d142a1b0e9c" />

11. Next on the `Connectivity` tab for `Compute resource`, select `Connect to an EC2 compute resource` and choose `nautilius-ec2` as instance and leave all other choices as `default`.

    <img width="1303" height="445" alt="image" src="https://github.com/user-attachments/assets/356894ea-f22b-442a-8a91-6d6dc2474c3e" />

12. Next on the `Additional configuration` tab, enter `nautilus_db` in the `Initial database name` under `Database option`.

    <img width="1303" height="318" alt="image" src="https://github.com/user-attachments/assets/73cc49db-e40e-476f-8554-6e791d6bc530" />

13. Now click on `Create database`

## Task 4: Copy `index.php` to `nautilius-ec2`

1. On the `aws-client`, change the `index.php` as follows:

   ```php
   <?php
    $dbname = 'nautilus_db';
    $dbuser = 'nautilus_admin';
    $dbpass = 'nautilus_db';
    $dbhost = 'nautilus-rds.cgmyyysfambv.us-east-1.rds.amazonaws.com';

    $link = mysqli_connect($dbhost, $dbuser, $dbpass) or die("Unable to Connect to '$dbhost'");
    mysqli_select_db($link, $dbname) or die("Could not open the db '$dbname'");

    $test_query = "SHOW TABLES FROM $dbname";
    $result = mysqli_query($link, $test_query);

    $tblCnt = 0;
    while($tbl = mysqli_fetch_array($result)) {
      $tblCnt++;
    }

    if (!$tblCnt) {
      echo "Connected successfully<br />\n";
    } else {
      echo "Connected successfully<br />\n";
    }
   ?>
   ```

2. Copy the `index.php` file to `nautilius-ec2` -> `/var/www/html/`

3. Give permission to `index.php` for apache

   ```sh
   sudo chown www-data:www-data /var/www/html/index.php
   sudo chmod 644 /var/www/html/index.php
   ```

4. Remove default apache welcome page.

   ```sh
   sudo rm /var/www/html/index.html
   ```

5. Restart apache

   ```sh
   sudo systemctl restart apache2
   ```

6. Browse `nautilius-vm` public ip and you should see:

   ```
   Connected successfully
   ```
