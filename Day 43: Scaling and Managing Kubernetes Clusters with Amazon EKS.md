# Day 43: Scaling and Managing Kubernetes Clusters with Amazon EKS

The Nautilus DevOps team has been tasked with preparing the infrastructure for a new Kubernetes-based application that will be deployed using Amazon EKS. The team is in the process of setting up an EKS cluster that meets their internal security and scalability standards. They require that the cluster be provisioned using the latest stable Kubernetes version to take advantage of new features and security improvements.

To minimize external exposure, the EKS cluster endpoint must be kept private. Additionally, the cluster needs to use the default VPC with availability zones `a`, `b`, and `c` to ensure high availability across different physical locations.

Your task is to create an EKS cluster named `datacenter-eks`, along with an IAM role for the cluster named `eksClusterRole`. The Kubernetes version must be `1.30`. Ensure that the cluster endpoint access is configured as private.

Finally, verify that the EKS cluster is successfully created with the correct configuration and is ready for workloads.



## Task 1: Create IAM role for EKS cluster

1. **Go to IAM dashboard and select `Role` from navigation menu.**

   <img width="233" height="427" alt="image" src="https://github.com/user-attachments/assets/5b86684e-f3aa-4c26-8733-91e2a3f6ddcc" />

2. **Click `Create role` and select `AWS service` for `Trusted entity type`.**

   <img width="1049" height="254" alt="image" src="https://github.com/user-attachments/assets/f72c6e34-7da8-4da0-b968-762e5b93e4f4" />

3. **Also select `EKS - cluster` under `Use case` section and click `Next` and go to step 3 `Name, review, and create`.**

   <img width="1049" height="404" alt="image" src="https://github.com/user-attachments/assets/e9df6a62-5fcc-4215-8701-63a3b2846c7f" />

4. **On the `Name, review, and create` step, enter cluster name.**

   <img width="1049" height="281" alt="image" src="https://github.com/user-attachments/assets/79968283-8771-4711-b550-66ca2cc62755" />

5. **Click on `Create role`.**

>[!Warning]
> **Create the cluster 20 min ahead of lab as EKS cluster takes time to create.**

## Task 2: Create EKS cluster

1. **Go to EKS dashboard and select `Clusters` from navigation menu and click `Create cluster`.**

   <img width="1364" height="362" alt="image" src="https://github.com/user-attachments/assets/18bedf97-649e-4740-b753-e9c4df347910" />

2. **For `Configuration options - new`, select `Custom configuration`.**

   <img width="797" height="166" alt="image" src="https://github.com/user-attachments/assets/efd01e92-35dc-4178-8a09-861781152acc" />

3. **Under `Cluster configuration` section, enter cluster name and IAM role `eksCLusterRole`.**

   <img width="797" height="313" alt="image" src="https://github.com/user-attachments/assets/0300f45d-76fc-433a-a2ae-e84d593354d9" />

5. **Next under `Kubernetes version settings` section, select version `1.30`.**

   <img width="797" height="292" alt="image" src="https://github.com/user-attachments/assets/2ed32e2b-9ba7-42ff-8a44-702afa4b5be9" />

6. **Click on `Next` and proceed to `Step 2: Specify networking`.**
7. **Under `Networking` section, select `default` VPC, security group and any three subnet.**

   <img width="856" height="441" alt="image" src="https://github.com/user-attachments/assets/53914fc9-41ac-4906-a943-fcf2dc86db82" />

8. **Next on `Cluster endpoint access` section, select option `Private`.**

   <img width="734" height="236" alt="image" src="https://github.com/user-attachments/assets/f06f85ec-cbe3-4f69-bcf4-0375e8d75524" />

9. **On `Step 4: Select add-ons` page, choose the add-ons that you want to add to your cluster.**

    <img width="901" height="523" alt="image" src="https://github.com/user-attachments/assets/c0166c77-f092-4abd-bae2-f7717346f5cc" />

    **`Leave add-on that are selected by default`**
   
    <img width="695" height="476" alt="image" src="https://github.com/user-attachments/assets/cf9b2581-27e7-4953-b28b-488b23cf68ac" />

11. **Proceed to `Step 6: Review and create` and review necessary steps.** 

    <img width="734" height="298" alt="image" src="https://github.com/user-attachments/assets/3a586f4f-76bc-4259-ac04-cce61ce3153d" />
    <img width="734" height="229" alt="image" src="https://github.com/user-attachments/assets/8e73e385-a143-4b4c-a35f-6ac654f920c7" />
    <img width="734" height="412" alt="image" src="https://github.com/user-attachments/assets/2a4245cb-191b-45e8-bda5-20337ff4feae" />
    <img width="734" height="411" alt="image" src="https://github.com/user-attachments/assets/7eb68c60-5ea0-4bc1-8363-a7f4b3eb25e8" />

12. **Click `Create cluster` and wait for the cluster `Status` appear as `Active`.**

    <img width="1018" height="223" alt="image" src="https://github.com/user-attachments/assets/4f7b62c3-f53d-4abc-a1d4-5ed464114eaf" />

>[!WARNING]
> Wait at least 5 minute after `Status` appears as `Active`.
