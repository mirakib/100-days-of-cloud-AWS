# Day 42: Building and Managing NoSQL Databases with AWS DynamoDB

The Nautilus DevOps team is developing a simple 'To-Do' application using DynamoDB to store and manage tasks efficiently. The team needs to create a DynamoDB table to hold tasks, each identified by a unique task ID. Each task will have a description and a status, which indicates the progress of the task (e.g., 'completed' or 'in-progress').

Your task is to:

- Create a **DynamoDB** table named `nautilus-tasks` with a primary key called `taskId` (string).
- Insert the following tasks into the table:
  
  - **Task 1**:
    
    taskId: '1'
    description: 'Learn DynamoDB'
    status: 'completed'
    
  - **Task 2**:
    
    taskId: '2', description:
    'Build To-Do App'
    status: 'in-progress'
    
- Verify that Task 1 has a status of '`completed`' and Task 2 has a status of '`in-progress`'.

Ensure the DynamoDB table is created successfully and that both tasks are inserted correctly with the appropriate statuses.


## Task 1: Create DynamoDB table

1. Go to DynamoDB dashboard and click `Create table`.

   <img width="337" height="288" alt="image" src="https://github.com/user-attachments/assets/534022ab-7496-40a0-ac85-875fac368e64" />

2. Enter name `nautilus-tasks` with a primary key called `taskId` (string).

   <img width="1303" height="382" alt="image" src="https://github.com/user-attachments/assets/4509a41f-20c8-4ca1-a02f-1bd494c48733" />

3. Click `Create table`.
4. Wait for the table status to be shown as `Active`.

   <img width="1086" height="167" alt="image" src="https://github.com/user-attachments/assets/2d81bc06-b324-4158-bb7f-3a83f8686fd3" />

## Task 2: Add items to table

1. To insert items to the table, click on the table and choose `Action` > `Create items`.

   <img width="1072" height="454" alt="image" src="https://github.com/user-attachments/assets/998de425-9ef2-4354-a3cb-0bc9440f0a48" />

2. Insert the following tasks into the table:

   <img width="1174" height="290" alt="image" src="https://github.com/user-attachments/assets/0725a1d7-64c0-404b-9ab9-51cbdbdbb619" />

   <img width="1179" height="298" alt="image" src="https://github.com/user-attachments/assets/c38cc725-59c0-4ee7-9c26-b911a584f491" />

3. From DynamoDB navigation pane, select `Explore items`.

   <img width="220" height="308" alt="image" src="https://github.com/user-attachments/assets/0ba975b6-61e0-4faf-b855-380fb0e6868d" />

2. Verify that Task 1 has a status of 'completed' and Task 2 has a status of 'in-progress'.

   <img width="769" height="212" alt="image" src="https://github.com/user-attachments/assets/e65f8a15-59a3-4675-9b15-6a7deb597490" />
