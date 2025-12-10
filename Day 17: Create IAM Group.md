# Day 17: Create IAM Group

Create an IAM group named `iamgroup_ammar`.

AWS Docs to be followed: [Create IAM groups](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_groups_create.html#id_groups_create-section-1)

## To create an IAM group and attach policies:

1. Sign in to the `AWS Management Console` and open the `IAM` console at https://console.aws.amazon.com/iam/

   <img width="984" height="401" alt="image" src="https://github.com/user-attachments/assets/8570a9e3-0f2d-4a68-b762-adf87237d8c1" />

3. In the navigation pane, choose `User groups` and then choose `Create group`.

   <img width="1354" height="450" alt="image" src="https://github.com/user-attachments/assets/4d733c36-224c-4621-ab8c-0b16e208616e" />

5. For `User group name`, type the name of the group.

   <img width="1052" height="229" alt="image" src="https://github.com/user-attachments/assets/e4a8c458-bfd8-4f30-83f9-987df06a93ed" />

   **Group names can be a combination of up to 128 letters, digits, and these characters: plus (+), equal (=), comma (,), period (.), at sign (@), underscore (_), and hyphen (-). Names must be unique within an account. They aren't distinguished by case. For example, you cannot create groups named both ADMINS and admins.**
7. In the list of users, select the check box for each user that you want to add to the group.
8. In the list of policies, select the check box for each policy that you want to apply to all members of the group.
9. Choose `Create group`.
