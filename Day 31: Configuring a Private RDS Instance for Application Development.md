# Day 31: Configuring a Private RDS Instance for Application Development

As a member of the Nautilus DevOps Team, your task is to perform the following:

- **Provision a Private RDS Instance**: Create a new private RDS instance named `datacenter-rds` using a `sandbox` template, further it must be a `db.t3.micro` type instance.
- **Engine Configuration**: Use the `MySQL` engine with version `8.4.x`.
- **Enable Storage Autoscaling**: Enable storage autoscaling and set the threshold value to `50GB`. Keep the rest of the configurations as default.
- **Instance Availability**: Ensure the instance is in the `available` state before submitting this task.

## Create a MySQL DB instance

1. Sign in to the **AWS Management Console** and open the **Amazon RDS console**
2. In the navigation pane, choose `Databases`.

   <img width="256" height="314" alt="image" src="https://github.com/user-attachments/assets/6873a4d0-f086-4520-be08-1a951c45561a" />

3. Choose `Create database` and make sure that `Easy create` is chosen.

   <img width="1299" height="148" alt="image" src="https://github.com/user-attachments/assets/82a2c6bb-da19-48b8-b100-8449402db571" />

4. In `Configuration`, choose `MySQL`.

   <img width="1299" height="413" alt="image" src="https://github.com/user-attachments/assets/95838069-9816-4e16-9a75-d459190ee676" />

5. For `DB instance size`, choose **Free tier**. **Free tier** appears for free plan accounts. **Sandbox** appears for paid plan accounts.

6. For DB instance identifier, enter `datacenter-rds`.

   <img width="1298" height="358" alt="image" src="https://github.com/user-attachments/assets/e3e7044c-c003-432c-b6a5-ff3834e9b8cd" />

7. Choose `Create database`.


## To enable storage autoscaling for a new DB instance

1. After creating database, select the database and click `Modify`
2. Change DB verion to `8.4.+`

     <img width="1299" height="140" alt="image" src="https://github.com/user-attachments/assets/1359f0a1-0ca9-44db-a934-6b0396420af6" />
     
3. In `Instance configuration` section, select instance type as `db.t3.micro`

     <img width="1299" height="328" alt="image" src="https://github.com/user-attachments/assets/d2aebe9a-0a64-48af-a444-ccc42277d48f" />
     
4. Under `Storage > Addiotional storage configuration`, enable storage autoscalling and set threshold `50GB`

     <img width="1299" height="265" alt="image" src="https://github.com/user-attachments/assets/44e258f4-73de-4bce-a290-b90bcb0bd0fe" />

5. Now at the bottom of the page, click `Continue` and check summary modification.

   <img width="1299" height="309" alt="image" src="https://github.com/user-attachments/assets/0414c33c-42ff-4b8b-9613-8512c3e7e1cc" />

6. Under `Scedule modification`, select `Apply immediately` and click `Modify DB instance`

   <img width="1321" height="264" alt="image" src="https://github.com/user-attachments/assets/1729acbc-8d00-40a6-90fd-af58f9317999" />

