# This demo shows the difference between a basic Terraform configuration and one that uses data sources and local values


- File structure

```bash
terraform-demo/
├── without-data-source-locals/
│   ├── main.tf
│   ├── variables.tf
│   ├── outputs.tf
│
├── with-data-source-locals/
│   ├── main.tf
│   ├── variables.tf
│   ├── locals.tf
│   ├── outputs.tf
│
└── README.md
```

##  1. Without Data Source and Locals

This version is simple but requires you to manually enter the AMI ID and repeat tag values.

- main.tf

```bash
provider "aws" {
  region = var.aws_region
}

resource "aws_instance" "example" {
  ami           = var.ami_id
  instance_type = var.instance_type
  tags = {
    Name = "Without-DataSource-Local"
  }
}
```
- variables.tf

```bash
variable "aws_region" {
  description = "AWS region to deploy the instance"
  type        = string
  default     = "us-east-1"
}

variable "ami_id" {
  description = "AMI ID for EC2 instance"
  type        = string
  default = "ami-0ff8a91507f77f867"
}

variable "instance_type" {
  description = "Instance type"
  type        = string
  default     = "t2.micro"
}
```

-  outputs.tf

```bash
output "instance_id" {
  description = "ID of the created EC2 instance"
  value       = aws_instance.example.id
}
```

###  2. With Data Source and Local Values


- Uses a data source to fetch the latest Amazon Linux 2 AMI.

- Uses local values to define common tags.

- main.tf

```bash
provider "aws" {
  region = var.aws_region
}

data "aws_ami" "amazon_linux" {
  most_recent = true
  owners      = ["amazon"]

  filter {
    name   = "name"
    values = ["amzn2-ami-hvm-*-x86_64-gp2"]
  }
}

resource "aws_instance" "example" {
  ami           = data.aws_ami.amazon_linux.id
  instance_type = local.common_tags.instance_type
  tags          = local.common_tags
}
```
- variables.tf

```bash
variable "aws_region" {
  description = "AWS region to deploy the instance"
  type        = string
  default     = "us-east-1"
}

variable "instance_type" {
  description = "Instance type"
  type        = string
  default     = "t2.micro"
}
```
- locals.tf

```bash
locals {
  common_tags = {
    Name        = "With-DataSource-Local"
    Environment = "Dev"
    Owner       = "BeginnerDemo"
    instance_type = "t2.micro"
  }
}
```
- outputs.tf

```bash
output "instance_id" {
  description = "ID of the created EC2 instance"
  value       = aws_instance.example.id
}

output "ami_used" {
  description = "The AMI ID used for the instance"
  value       = data.aws_ami.amazon_linux.id
}
output "tags" {
  description = "Tags applied to the EC2 instance"
  value       = local.common_tags.instance_type
  
}
```
