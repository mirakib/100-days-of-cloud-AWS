✅ Method 1: Create Key Pair via AWS Management Console

- Log in using the provided console URL.

- Ensure the region at the top right is set to **N. Virginia (us-east-1)**.

- In the console search bar, type EC2 and open the EC2 dashboard.

  <img width="1366" height="615" alt="image" src="https://github.com/user-attachments/assets/a3c3bb49-1336-43b2-803e-7013a5bc3ea2" />

- In the left sidebar, scroll down to **“Network & Security”** → **“Key Pairs”**.

  <img width="1366" height="615" alt="image" src="https://github.com/user-attachments/assets/215d6651-cef3-4793-b574-b7db458d7820" />


- **Click** “Create key pair”.

- Fill the form:

    - Name: `datacenter-kp`

    - Key pair type: `RSA`

    - Private key file format:

        - Choose `.pem` if you will use SSH directly

        - Choose `.ppk` if you plan to SSH using PuTTY
          
  <img width="1366" height="615" alt="image" src="https://github.com/user-attachments/assets/8f75c313-d391-44b1-b654-68d3bdb4d5b5" />


- Click **“Create key pair”.**

- The private key file will download automatically. Save it securely.
