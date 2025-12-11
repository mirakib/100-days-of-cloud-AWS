# Day 18: Create Read-Only IAM Policy for EC2 Console Access

Create an IAM policy named `iampolicy_anita` in `us-east-1` region, it must allow read-only access to the EC2 console, i.e this policy must allow users to view all instances, AMIs, and snapshots in the Amazon EC2 console.

AWS DOcs to be followed: [Create IAM policies (console)](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_create-console.html#access_policies_create-json-editor)

## To use the JSON policy editor to create a policy

1. Sign in to the `AWS Management Console` and open the IAM console at [https://console.aws.amazon.com/iam/](https://console.aws.amazon.com/iam/)

   <img width="987" height="446" alt="image" src="https://github.com/user-attachments/assets/f64ff977-879b-4676-8b34-9b3257915b50" />

3. In the navigation pane on the left, choose `Policies`.

   <img width="263" height="466" alt="image" src="https://github.com/user-attachments/assets/3d69cac5-ce9b-4cd9-bea3-25bab85bda2a" />

5. Choose `Create policy`.

   <img width="1054" height="141" alt="image" src="https://github.com/user-attachments/assets/6793a244-acef-4e73-9153-968d0b9e35fc" />

7. In the `Policy editor` section, choose the `Visual` option and select `EC2`.

   <img width="1328" height="337" alt="image" src="https://github.com/user-attachments/assets/84dfb468-d5ee-4792-b729-519b250b4c9a" />

9. In Actions allowed, choose the actions to add to the policy. You can choose actions in the following ways:

    - Select the checkbox for all actions.
    - Choose add actions to type the name of a specific action. You can use wildcards (*) to specify multiple actions.
    - Select one of the Access level groups to choose all actions for the access level (for example, Read, Write, or List).
    - Expand each of the Access level groups to choose individual actions.
10. On the `Review and create` page, type a `Policy Name` and a `Description (optional)` for the policy that you are creating. Review the Permissions defined in this policy to make sure that you have granted the intended permissions.

    <img width="1322" height="421" alt="image" src="https://github.com/user-attachments/assets/1bbf9632-bd60-46d2-82dd-b99caa867ed1" />

14. Choose Create policy to save your new policy.

    

