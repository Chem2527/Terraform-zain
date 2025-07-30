# Usage of Aws vault and why we need to use?
- In traditional way we will be storing aws creds under .aws/credentials/  in plain text.. anyone who hacks our system can read values..
- **Windows Credential Manager** is a built-in secure storage system on Windows that encrypts and protects sensitive credentials like passwords, certificates, and other secrets.
- Aws vault will also use this feature which is secure.
- Install aws-vault
  ```bash
  choco install aws-vault
  ```
  - add your environment profiles
  ```bash
  aws-vault add dev # (provide your aws access and secret keys)
  ```
- apply terraform using profiles
```bash
 aws-vault exec dev -- terraform apply
```
