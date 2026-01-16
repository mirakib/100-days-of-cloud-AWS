# Day 50: Expanding EC2 Instance Storage for Development Needs

The Nautilus DevOps Team has recently been informed by the Development Team that their EC2 instance is running out of storage space. This instance, crucial for development activities, is named nautilus-ec2 and currently has an attached volume of 8 GiB. To accommodate the increasing data requirements, the storage needs to be expanded to 12 GiB. This change should ensure that the expanded space is immediately available for use within the instance without disrupting ongoing activities.

- **Identify Volume**: Find the volume attached to the `nautilus-ec2` instance.

- **Expand Volume**: Increase the volume size from `8 GiB` to `12 GiB`.

- **Reflect Changes**: Ensure the root (`/`) partition within the instance reflects the expanded size from `8 GiB` to `12 GiB`.

- **SSH Access**: Use the key pair located at `/root/nautilus-keypair.pem` on the `aws-client` host to `SSH` into the EC2 instance.

## Task 1:  Find the volume attached

1. From EC2 dashboard, click on the instance and select `Storage` tab.

   <img width="1058" height="353" alt="image" src="https://github.com/user-attachments/assets/7f4559de-aba7-41a9-8ed7-6eae83d6fd3a" />

2. Select the `Volume` and preview its details.

   <img width="1058" height="386" alt="image" src="https://github.com/user-attachments/assets/8c860ac3-d506-4a29-bd96-74620c0f11e5" />

## Task 2: Expand the volume

1. Select the volume and from `Action`, choose `Modify volume`.

   <img width="1108" height="476" alt="image" src="https://github.com/user-attachments/assets/0ee34010-d432-4d94-88ce-708b93e32eaf" />

2. Enter new size and click modify.

   <img width="1303" height="428" alt="image" src="https://github.com/user-attachments/assets/af83c7da-8133-4ed6-9a49-254cd4fa47aa" />

3. After updating volume size, it state should appear as `In-use - optimizing (0%)`.

   <img width="1088" height="163" alt="image" src="https://github.com/user-attachments/assets/ecc44795-64ed-47bd-a366-86369344c0d5" />

   
## Task 3:

1. **SSH into the VM:**

   ```sh
   cp nautilus-keypair.pem .ssh/id_rsa
   ```

   ```sh
   ssh ec2-user@<nautilus-vm-pub-ip>
   ```

>[!Note]
> **After you increase the size of an EBS volume, you must extend the partition and file system to the new, larger size. You can do this as soon as the volume enters the optimizing state.**

**AWS Doc to be followed: [Extend the file system after resizing an Amazon EBS volume](https://docs.aws.amazon.com/ebs/latest/userguide/recognize-expanded-volume-linux.html)**

2. **Extend the file system of EBS volumes:**

   **Check whether the volume has a partition:**

   ```sh
   sudo lsblk
   ```
   
   ```
   NAME      MAJ:MIN RM SIZE RO TYPE MOUNTPOINTS
   xvda      202:0    0  12G  0 disk 
   ├─xvda1   202:1    0   8G  0 part /
   ├─xvda127 259:0    0   1M  0 part 
   └─xvda128 259:1    0  10M  0 part /boot/efi
   ```

   ```sh
   df -Th
   ```

   **The following example output for the `df -hT` command shows that the `/dev/xvda1` file system is `8 GB` in size, its type is `ext4`, and its mount point is `/`.**

   ```
   Filesystem     Type      Size  Used Avail Use% Mounted on
   devtmpfs       devtmpfs  4.0M     0  4.0M   0% /dev
   tmpfs          tmpfs     475M     0  475M   0% /dev/shm
   tmpfs          tmpfs     190M  2.9M  188M   2% /run
   /dev/xvda1     xfs       8.0G  1.6G  6.5G  19% /
   tmpfs          tmpfs     475M     0  475M   0% /tmp
   /dev/xvda128   vfat       10M  1.3M  8.7M  13% /boot/efi
   tmpfs          tmpfs      95M     0   95M   0% /run/user/1000
   ```

   **To extend a partition named `xvda1`, use the following command:**
   
   ```sh
   sudo growpart /dev/xvda 1
   ```

   ```
   CHANGED: partition=1 start=24576 old: size=16752607 end=16777183 new: size=25141215 end=25165791
   ```

   ```sh
   sudo lsblk
   ```
   
   ```
   NAME      MAJ:MIN RM SIZE RO TYPE MOUNTPOINTS
   xvda      202:0    0  12G  0 disk 
   ├─xvda1   202:1    0  12G  0 part /
   ├─xvda127 259:0    0   1M  0 part 
   └─xvda128 259:1    0  10M  0 part /boot/efi
   ```

   ```sh
   df -hT
   ```

   ```
   Filesystem     Type      Size  Used Avail Use% Mounted on
   devtmpfs       devtmpfs  4.0M     0  4.0M   0% /dev
   tmpfs          tmpfs     475M     0  475M   0% /dev/shm
   tmpfs          tmpfs     190M  2.9M  188M   2% /run
   /dev/xvda1     xfs       8.0G  1.6G  6.5G  19% /
   tmpfs          tmpfs     475M     0  475M   0% /tmp
   /dev/xvda128   vfat       10M  1.3M  8.7M  13% /boot/efi
   tmpfs          tmpfs      95M     0   95M   0% /run/user/1000
   ```

   **Extend a file system mounted on `/`, use the following command:**
   
   ```sh
   sudo xfs_growfs -d /
   ```

   ```
   meta-data=/dev/xvda1             isize=512    agcount=2, agsize=1047040 blks
         =                       sectsz=4096  attr=2, projid32bit=1
         =                       crc=1        finobt=1, sparse=1, rmapbt=0
         =                       reflink=1    bigtime=1 inobtcount=1
   data     =                       bsize=4096   blocks=2094075, imaxpct=25
         =                       sunit=128    swidth=128 blks
   naming   =version 2              bsize=16384  ascii-ci=0, ftype=1
   log      =internal log           bsize=4096   blocks=16384, version=2
         =                       sectsz=4096  sunit=4 blks, lazy-count=1
   realtime =none                   extsz=4096   blocks=0, rtextents=0
   data blocks changed from 2094075 to 3142651
   ```

   ```sh
   df -hT
   ```

   ```
   Filesystem     Type      Size  Used Avail Use% Mounted on
   devtmpfs       devtmpfs  4.0M     0  4.0M   0% /dev
   tmpfs          tmpfs     475M     0  475M   0% /dev/shm
   tmpfs          tmpfs     190M  2.9M  188M   2% /run
   /dev/xvda1     xfs        12G  1.6G   11G  13% /
   tmpfs          tmpfs     475M     0  475M   0% /tmp
   /dev/xvda128   vfat       10M  1.3M  8.7M  13% /boot/efi
   tmpfs          tmpfs      95M     0   95M   0% /run/user/1000
   ```

   <img width="1088" height="170" alt="image" src="https://github.com/user-attachments/assets/2840d500-9cd9-485a-adb8-e112c8b5694a" />
