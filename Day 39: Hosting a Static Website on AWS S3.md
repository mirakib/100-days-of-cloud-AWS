# Day 39: Hosting a Static Website on AWS S3

The Nautilus DevOps team has been tasked with creating an internal information portal for public access. As part of this project, they need to host a static website on AWS using an S3 bucket. The S3 bucket must be configured for public access to allow external users to access the static website directly via the S3 website URL.

Task Requirements:

- Create an S3 bucket named `nautilus-web-13898`.
- Configure the S3 bucket for static website hosting with `index.html` as the index document.
- Allow public access to the bucket so that the website is publicly accessible.
- Upload the `index.html` file from the `/root/` directory of the `AWS client` host to the S3 bucket.
- Verify that the website is accessible directly through the S3 website URL.

## Task 1: Create an S3 bucket named `nautilus-web-13898`.

1. Go to S3 Bucket page and click `Create bucket`.

   <img width="310" height="232" alt="image" src="https://github.com/user-attachments/assets/030ffa9f-f445-4a99-bfc3-6f5205e3441e" />

2. Enter the Bucket name and choose region.

   <img width="1303" height="374" alt="image" src="https://github.com/user-attachments/assets/06a137c6-f733-4000-b5d5-d1f9c4498a59" />

3. Under `Block Public Access settings for this bucket` section, uncheck `Block all public access`.

   <img width="1299" height="378" alt="image" src="https://github.com/user-attachments/assets/4ea4bff4-4e1d-4547-8eb8-de628a810773" />

5. To accept the default settings and create the bucket, choose `Create`.

## Task 2: Enable static website hosting

1. Sign in to the AWS Management Console and open the Amazon S3 console at [https://console.aws.amazon.com/s3](https://console.aws.amazon.com/s3)
2. In the left navigation pane, choose `General purpose buckets`.

   <img width="255" height="201" alt="image" src="https://github.com/user-attachments/assets/f8fe8fd9-94ee-443a-969f-93394b245b1d" />

3. In the buckets list, choose the name of the bucket that you want to enable static website hosting for.

   <img width="1018" height="183" alt="image" src="https://github.com/user-attachments/assets/470b062f-d614-4ba3-b9b4-e15b290bd6c8" />

4. Choose `Properties`.

   <img width="1064" height="251" alt="image" src="https://github.com/user-attachments/assets/86647276-c474-49fc-a7b7-f479fe059edd" />

5. Under `Static website hosting`, choose `Edit`.

   <img width="1018" height="251" alt="image" src="https://github.com/user-attachments/assets/aed00f2f-c530-4e3f-b288-c51f5869b8b1" />

6. `Under Static website hosting`, choose `Enable`.

    <img width="1018" height="282" alt="image" src="https://github.com/user-attachments/assets/7d34651b-ba41-4e6f-b90d-1a523cb01896" />

7. In `Index document`, enter the file name of the index document, typically `index.html`.

   <img width="1010" height="183" alt="image" src="https://github.com/user-attachments/assets/385ebfcb-06a7-4fb3-b8db-9e2408948345" />

8. Choose `Save changes`.

9. Next under the `Permissions` tab, edit `Bucket policy` as following:

    ```json
    {
      "Version": "2012-10-17",
      "Statement": [
        {
          "Sid": "PublicReadGetObject",
          "Effect": "Allow",
          "Principal": "*",
          "Action": "s3:GetObject",
          "Resource": "arn:aws:s3:::nautilus-web-13898/*"
        }
      ]
    }
    ```

## Task 3: Upload index.html from aws client host to bucket

1. Uploads file via CLI from aws client host.

   ```sh
   aws s3 cp index.html s3://nautilus-web-13898/
   ```

2. Verify the object is now in the bucket.

   <img width="1018" height="294" alt="image" src="https://github.com/user-attachments/assets/b6464682-0eaf-4fe4-99d5-d29cdf578d37" />

## Task 4: Access the application

1. Select `Properties` from the bucket.

   <img width="1056" height="118" alt="image" src="https://github.com/user-attachments/assets/8a9c25a0-8b8a-4bea-897a-acc5bfe6f56f" />

2. At the `Static webiste hosting` section, click on the `Bucket website endpoint`

   <img width="1018" height="391" alt="image" src="https://github.com/user-attachments/assets/48d12120-ec3d-4011-a39a-a65eb817f8e7" />

3. The `Welcome to KKE labs!` text will apear as a web page.

   <img width="946" height="131" alt="image" src="https://github.com/user-attachments/assets/a99ed2f1-5511-46e0-bad1-0ee74a67546e" />
