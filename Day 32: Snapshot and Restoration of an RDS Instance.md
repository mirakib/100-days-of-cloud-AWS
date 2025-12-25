# Day 32: Snapshot and Restoration of an RDS Instance

The Nautilus Development Team is preparing for a major update to their database infrastructure. To ensure a smooth transition and to safeguard data, the team has requested the DevOps team to take a snapshot of the current RDS instance and restore it to a new instance. This process is crucial for testing and validation purposes before the update is rolled out to the production environment. The snapshot will serve as a backup, and the new instance will be used to verify that the backup process works correctly and that the application can function seamlessly with the restored data.

As a member of the Nautilus DevOps Team, your task is to perform the following:

- **Take a Snapshot**: Take a snapshot of the `devops-rds` RDS instance and name it `devops-snapshot` (please wait `devops-rds` instance to be in available state).
- **Restore the Snapshot**: Restore the snapshot to a new RDS instance named `devops-snapshot-restore`.
- **Configure the New RDS Instance**: Ensure that the new RDS instance has a class of `db.t3.micro`.
- **Verify the New RDS Instance**: The new RDS instance must be in the `Available` state upon completion of the restoration process.

## Task 1: To create a DB snapshot

1. Sign in to the AWS Management Console and open the Amazon RDS console
2. In the navigation pane, choose `Snapshots`.

   <img width="263" height="314" alt="image" src="https://github.com/user-attachments/assets/de9c24dc-243e-42e0-9745-0faba2576bdb" />

4. Choose `Take snapshot`.

   <img width="1068" height="334" alt="image" src="https://github.com/user-attachments/assets/2329f963-1eb7-48cc-9abd-a962dc3885bb" />

5. Choose the `DB instance` for which you want to take a snapshot. Enter the `Snapshot name`. Choose `Take snapshot`.

   <img width="1353" height="454" alt="image" src="https://github.com/user-attachments/assets/0828badd-0016-48fa-8857-a97f366240a5" />

6. The Manual snapshots list appears, wait for the new DB snapshot's status to be shown as `Available`. 

   <img width="1088" height="257" alt="image" src="https://github.com/user-attachments/assets/2c40475c-32f5-4343-9cca-c5b0fa92bf2a" />

## Task 2: Launch new RDS instance from snapshot.

1. From the AWS Management Console, open the Amazon RDS console.
2. In the navigation pane, choose `Snapshots`.
3. Select the snapshot and click `Action > Restore snapshot`

   <img width="1088" height="257" alt="image" src="https://github.com/user-attachments/assets/3e2b82aa-3992-4f77-a9ed-6ec71c9cf747" />

4. Under `Settings`, enter instance name.

   <img width="1303" height="219" alt="image" src="https://github.com/user-attachments/assets/67a80979-9b24-4e82-92bb-665f83efb298" />

5. Under `Instance configuration`, select `Burstable classes` and `db.t3.micro`

   <img width="1303" height="293" alt="image" src="https://github.com/user-attachments/assets/5ded375b-1ea2-4e58-808a-a891657c8072" />

6. Leave all other as default and click `Restore DB instance`
7. Wait for `devops-snapshot-restore` instance status to be `Available`.

   <img width="1081" height="215" alt="image" src="https://github.com/user-attachments/assets/1789d180-11a1-443c-96db-7f3805cd1284" />
