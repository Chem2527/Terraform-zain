# State Locking using S3 and storing state file remotely in s3

- What is .tfstate?

- When Terraform applies your configuration, it keeps track of the resources it manages in a file called **.tfstate**

- Terraform compares the current state with your config to decide what to create/update/destroy.

```bash
| Feature       | Local State                        | Remote State (S3)                        |
| ------------- | ---------------------------------- | ---------------------------------------- |
| Storage       | `.tfstate` on your machine         | Stored in S3 bucket                      |
| Collaboration |  Risky – multiple users overwrite  |  Safe with state locking (via DynamoDB)  |
| Safety        |  Easily lost                       |  Backups, versioning, centralized        |
```
- main.tf

```bash
terraform {
  backend "s3" {
    bucket = "mybucketlockbucket2527"
    key    = "dev/terraform.tfstate"
    region = "us-east-1"
    use_lockfile = true
  }
}

resource "aws_instance" "example" {
  ami           = "ami-08a6efd148b1f7504"
  instance_type = "t3.micro"

  tags = {
    Name = "ExampleInstance"
  }
}
```

- Destroy the infra which we created using terraform destroy now our remote state file in s3 is empty.

- remove .terraform/  & .hcl locally, 

```bash
rm -rf .terraform/
rm .terraform.lock.hcl
```
- run **terraform plan** and   understand whether it will add or delete resources (in our case ec2)?
