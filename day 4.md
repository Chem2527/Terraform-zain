# Terraform EC2 Demo: Static vs Dynamic (`for_each`) Approach

This guide demonstrates two approaches for launching multiple EC2 instances using Terraform:

- **Set 1**: Manual resource blocks
- **Set 2**: Dynamic configuration using `for_each`



## Set 1: WITHOUT `for_each` (Manual Approach)
- Define each EC2 instance separately using individual `aws_instance` resource blocks.

### Folder Structure

```bash
static/
├── main.tf
└── outputs.tf
```


- main.tf

```bash
provider "aws" {
  region = "us-east-1"
  
}

resource "aws_instance" "web1" {
  ami = "ami-020cba7c55df1f615"
  instance_type = "t3.micro"

}

resource "aws_instance" "web2" {
  ami = "ami-020cba7c55df1f615"
  instance_type = "t3.micro"

}
```
- outputs.tf

```bash
output "web1" {
  value = aws_instance.web1.public_ip
}

output "web2" {
  value = aws_instance.web2.public_ip
}


```
- **Disadvantages**
- Repetition: Code is duplicated for each instance.

- Scalability issue: Hard to manage when you need 5, 10, or more instances.

- Maintenance burden: Updating configurations for multiple instances is time-consuming.

## Set 2: WITH for_each (Dynamic Approach)

- Loop over this map and for each key-value pair, create a separate EC2 instance with the specified configuration

Folder Structure

```bash

foreach/
├── main.tf
├── variables.tf
└── outputs.tf
```
- main.tf

```bash
provider "aws" {
  region = "us-east-1"
  
}

resource "aws_instance" "ec2" {
  for_each = var.saikrishna

  ami           = each.value.ami_id
  instance_type = each.value.instance_type
  tags = {
    Name = each.key
  }
}
```
- variables.tf
```bash
variable "saikrishna" {

  description = "variable name dummy"
  type = map(object({
    instance_type = string
    ami_id = string
  }))
  default = {
    web1 = {
      instance_type = "t3.micro"
      ami_id = "ami-020cba7c55df1f615"
    }
    web2 = {
      instance_type = "t3.micro"
      ami_id = "ami-020cba7c55df1f615"
    }
  }
}


```
- outputs.tf

```bash
output "name" {
  value = {
    for instance in aws_instance.ec2 : instance.id => instance.tags.Name
  }
  
}

```
**Advantages**

- Scalable: Easily add or remove instances by modifying the map.

- Clean & DRY: No code duplication; centralized configuration.

- Dynamic properties: Configure instance types, AMIs, and tags per instance with ease.
