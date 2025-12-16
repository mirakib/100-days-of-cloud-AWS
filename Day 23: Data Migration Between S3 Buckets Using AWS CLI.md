# Day 23: Data Migration Between S3 Buckets Using AWS CLI

- **Create a New Private S3 Bucket**: Name the bucket `xfusion-sync-5262`.

- **Data Migration**: Migrate the entire data from the existing `xfusion-s3-29481` bucket to the new `xfusion-sync-5262` bucket.

- **Ensure Data Consistency**: Ensure that both buckets have the same data.

- **Use AWS CLI**: Use the AWS CLI to perform the creation and data migration tasks.


1. Check existing `S3` buckets.

   ```bash
   aws s3 ls
   ```

2. Create a private S3 bucket `xfusion-sync-5262`

   ```bash
   aws s3api create-bucket \
    --bucket xfusion-sync-5262 \
    --region us-east-1
    ```

3. Migrate data from old bucket to new bucket

   ```bash
   aws s3 sync s3://xfusion-s3-29481 s3://xfusion-sync-5262
   ```

4. Ensure data consistency

   ```bash
   aws s3 ls s3://xfusion-s3-29481 --recursive | wc -l
   aws s3 ls s3://xfusion-sync-5262 --recursive | wc -l
   ```
      
