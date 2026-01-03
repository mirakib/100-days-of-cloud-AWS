# Day 41: Securing Data with AWS KMS

The Nautilus DevOps team is focusing on improving their data security by using AWS KMS. Your task is to create a KMS key and manage the encryption and decryption of a pre-existing sensitive file using the KMS key.

Specific Requirements:

- Create a symmetric KMS key named `devops-KMS-Key` to manage encryption and decryption.
- Encrypt the provided `SensitiveData.txt` file (located in `/root/`), base64 encode the ciphertext, and save the encrypted version as `EncryptedData.bin` in the `/root/` directory.
- Try to decrypt the same and verify that the decrypted data matches the original file.

Make sure that the KMS key is correctly configured. The validation script will test your configuration by decrypting the `EncryptedData.bin` file using the KMS key you created.


## Task 1: Create a symmetric encryption KMS key with AWS console

1. Sign in to the AWS Management Console and open the AWS Key Management Service (AWS KMS)
2. In the navigation pane, choose `Customer managed keys`.

   <img width="250" height="202" alt="image" src="https://github.com/user-attachments/assets/ad685ed2-f671-453c-95d2-5a74abb76ac6" />

4. Choose `Create key`.
5. To create a symmetric encryption KMS key, for `Key type` choose `Symmetric`.

   <img width="1049" height="132" alt="image" src="https://github.com/user-attachments/assets/1b98ccc8-4811-4fb5-9a2d-bb0e942ea0a4" />

7. In `Key usage`, the `Encrypt and decrypt` option is selected for you.

   <img width="1049" height="118" alt="image" src="https://github.com/user-attachments/assets/862b4e5a-56a6-439e-aec2-34f47fba7ec4" />

8. Choose `Next`. Type an alias for the KMS key.

   <img width="1049" height="136" alt="image" src="https://github.com/user-attachments/assets/e6a01cbe-722f-45ec-99a2-9fc173b9aaa1" />

9. Review the key settings that you chose.

    <img width="1328" height="446" alt="image" src="https://github.com/user-attachments/assets/96a8be29-0672-4593-84ef-47e487903d15" />

10. Choose Finish to create the KMS key.

    <img width="481" height="53" alt="image" src="https://github.com/user-attachments/assets/701be48d-4d8b-44c6-9536-018f91fb00a8" />

## (Optional) Create symmetric encryption KMS key with CLI

1. Create a symmetric KMS key

   ```sh
   aws kms create-key \
    --description "devops-KMS-Key" \
    --key-usage ENCRYPT_DECRYPT \
    --key-spec SYMMETRIC_DEFAULT
   ```

>[!Note]
> Look for in the output `KeyId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx`

## Task 2: Encrypt the sensitive file usling CLI

Encrypt + Base64 encode output

```sh
aws kms encrypt \
  --key-id alias/devops-KMS-Key \
  --plaintext fileb:///root/SensitiveData.txt \
  --output text \
  --query CiphertextBlob | base64 --decode > /root/EncryptedData.bin
```

>[!Note]
> This creates `/root/EncryptedData.bin`

## Task 3: Decrypt the encrypted file

```sh
aws kms decrypt \
  --ciphertext-blob fileb:///root/EncryptedData.bin \
  --output text \
  --query Plaintext | base64 --decode > /root/DecryptedData.txt
```

## Task 4: Verify decrypted data matches original

```sh
diff /root/SensitiveData.txt /root/DecryptedData.txt
```

>[!Note]
> No output confirms successful encryption & decryption.
