# Understanding Terraform Import and its importance 

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
  instance_type = "t2.micro"

  tags = {
    Name = "ExampleInstance"
  }
}
```
- Delete the resource block of ec2 and keep empty or dummy values and delete .terraform,hcl


- Try to import the existing resources from gui to cli using below command

```bash
terraform import aws_instance.example <instance id>
```

- Try above command and see what happens.

- Then delete state file and lock remotely and try the command again
```bash
terraform import aws_instance.example <instance-id>
```
```bash
terraform state list # shows aws_instance.example
```
```bash
terraform state show aws_instance.example
```
- Match the existing infra configs and do the check using terraform plan



