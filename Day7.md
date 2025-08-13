
# Terraform AWS EC2 Deployment (Multi-Environment)

```bash
|-- dev.tfvars
|-- main.tf
|-- outputs.tf
|-- prod.tfvars
|-- test.tfvars
|-- variables.tf
```
```bash
aws configure --profile dev-profile
aws configure --profile test-profile
aws configure --profile prod-profile
```
- Each profile will be saved in your ~/.aws/credentials file.


-  main.tf

```bash
provider "aws" {
  region  = "us-east-1"
  profile = lookup(
    {
      dev  = "dev-profile"
      test = "test-profile"
      prod = "prod-profile"
    },
    terraform.workspace,
    "default"
  )
}
resource "aws_instance" "saikrishna" {
  ami           = var.ami_id
  instance_type = var.instance_type

  tags = {
    Name        = "saikrishna-${terraform.workspace}"
    environment = terraform.workspace
  }
}

```
- variables.tf
```bash
variable "ami_id" {
  description = "AMI ID for the AWS instance"
  type        = string
  default = "ami-0de716d6197524dd9"
  
}
variable "instance_type" {
  description = "Instance type for the AWS instance"
  type        = string
  default     = "t3.micro"
  
}
variable "tag_name" {
  description = "Tag name for the AWS instance"
  type        = string
  default     = "saikrishna-instance"
  
}
```

- dev.tfvars
```bash
ami_id = "ami-0de716d6197524dd9"
instance_type = "t2.nano"
```
- test.tfvars
```bash
ami_id = "ami-0de716d6197524dd9"
instance_type = "t2.micro"
```
- prod.tfvars
```bash
ami_id = "ami-0de716d6197524dd9"
instance_type = "t2.small"
```

## 1. Initialize Terraform

```bash
terraform init
```
## 2. Create Workspaces
```bash
terraform workspace new dev
terraform workspace new test
terraform workspace new prod
```
## 3. Apply Configuration

- Deploy to dev

```bash
terraform workspace select dev
terraform apply -var-file=dev.tfvars
```
- Deploy to test
```bash
terraform workspace select test
terraform apply -var-file=test.tfvars
```
- Do similarly for prod


- Destroy Resources

```bash
terraform workspace select dev  
terraform destroy -var-file=dev.tfvars
```
